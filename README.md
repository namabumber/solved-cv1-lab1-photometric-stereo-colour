Download Link: https://assignmentchef.com/product/solved-cv1-lab1-photometric-stereo-colour
<br>
<h1><a name="_Toc8818"></a>1             Photometric Stereo</h1>

In this part of the assignment, you are going to implement the photometric stereo algorithm as described in Section 5.4 (Forsyth and Ponce, <em>Computer Vision: A Modern Approach</em>). The chapter snippet can be found in the course materials.

Following this instruction, you will have to edit and fill in your code in the files <strong>estimate alb nrm.py</strong>, <strong>check integrability.py</strong>, and <strong>construct surface.py</strong>. The main script <strong>photometric stereo.py </strong>is provided for reference and should not be taken as is. Throughout the assignment, you will be asked to perform different trials and experiments which will require you to adjust the main code accordingly, this also shows how well you can cope with the materials.

Include images of the results into your report. For 3D models, make sure to choose a viewpoint that makes the structure as clear as possible and/or feel free to take them from multiple viewpoints.

<h2><a name="_Toc8819"></a>1.1           Estimating Albedo and Surface Normal</h2>

Let us start with the grayscale sphere model, which is located in the SphereGray5 folder. The folder contains 5 images of a sphere with grayscale checker texture under similar lighting conditions with the one in the book. Your task is to estimate the surface reflectance (albedo) and surface normal of this model. The light source directions are encoded in the image file names.

<h4>Hint</h4>

To get the least-squares solution of a linear system, you can use numpy.linalg.lstsq function.

<h4>Question – 1 (15-<em>pts</em>)</h4>

<ol>

 <li>Complete the code in <strong>estimate alb norm.py </strong>to estimate albedo and surface normal map for the SphereGray5 What do you expect to see in albedo image and how is it different with your result?</li>

 <li>In principle, what is the minimum number of images you need to estimate albedo and surface normal? Run the algorithm with more images by using SphereGray25 and observe the differences in the results. You could try all images at once or a few at the time, in an incremental fashion. Choose a strategy and justify it by discussing your results.</li>

 <li>What is the impact of shadows in photometric stereo? Explain the trick that is used in the text to deal with shadows. Remove that trick and check your results. Is the trick necessary in the case of 5 images, how about 25 images?</li>

</ol>

<h2><a name="_Toc8820"></a>1.2           Test of Integrability</h2>

Before we can reconstruct the surface height map, it is required to compute the partial derivatives and (or p and q in the algorithm). The partial derivatives also give us a chance to double check our computation, namely the test of <em>integrability</em>.

<h4>Question – 2 (10-<em>pts</em>)</h4>

<ol>

 <li>Compute the partial derivatives (p and q in the algorithm) by filling in your code into <strong>check integrability.py</strong>.</li>

 <li>Implement and compute the second derivatives according to the algorithm and perform the test of integrability by choosing a reasonable threshold. What could be the reasons for the errors? How does the test perform with different number of images used in the reconstruction process in Question-1?</li>

</ol>

<h2><a name="_Toc8821"></a>1.3           Shape by Integration</h2>

To reconstruct the surface height map, we need to continuously integrate the partial derivatives over a path. However, as we are working with discrete structures, you will be simply summing their values.

The algorithm in the chapter presents a way to do the integration in <em>column-major </em>order, that is you start at the top-left corner and integrate along the first column, then go towards right along each row. Yet, it is also noticed that it would be better to use many different paths and average so as to spread around the errors in the derivative estimates.

<strong>Note: </strong>By default, Numpy used row-major operations. So if you are unrolling an image to linearize the operation, you will end up with a row-major representation. Numpy can be configured to be column-major. Otherwise, if you are using the double for-loops without an unrolling operation, then this concern doesn’t apply.

<h4>Question – 3 (10-<em>pts</em>)</h4>

<ol>

 <li>Construct the surface height map using column-major order as described in the algorithm, then implement row-major path integration. Your code should now go to <strong>construct surface.py</strong>. What are the differences in the results of the two paths?</li>

 <li>Now, take the average of the results. Do you see any improvement compared to when using only one path? Are the construction results different with different number of images being used?</li>

</ol>

<h4>Hint</h4>

You could further inspect the shape of the objects and normal directions by using matplotlib.pyplot.quiver function. You will have to choose appropriate sub-sampling ratios for proper illustration. You code goes to the show results function in <strong>utils.py</strong>.

<h2><a name="_Toc8822"></a>1.4           Experiments with different objects</h2>

In this part, you will try to run the photometric stereo algorithm in various number of scenarios to see how well it can be generalized.

<h4>Question – 4 (5-<em>pts</em>)</h4>

Run the algorithm and show the results for the MonkeyGray model. The albedo results of the monkey may comprise more albedo errors than in case of the sphere. Observe and describe the errors. What could be the reason for those errors? You may want to experiment with different number of images as you did in Question-1 to see the effects. How do you think that could help solving these errors?

So far, we have assumed that albedos are 1-channel grayscale images and that input images are also 1-channel. To work with 3-channel images, a simple solution is to split the input image into separate channels and treat them individually. Yet, that would generate a small problem while constructing the surface normal map if a pixel value in a channel is zero.

<h4>Question – 5 (5-<em>pts</em>)</h4>

Update the implementation to work for 3-channel RGB inputs and test it with 2 models SphereColor and MonkeyColor. Explain your changes and show your results. Observe the problem in the constructed surface normal map and height map, explain why a zero pixel could be a problem and propose a way to overcome that.

Now, it is the time to try the algorithm on real-world datasets. For that purpose, we are going to use the <em>Yale Face Database</em><sup>1</sup>.

<a href="http://cvc.cs.yale.edu/cvc/projects/yalefaces/yalefaces.html">http://cvc.cs.yale.edu/cvc/projects/yalefaces/yalefaces.html</a>

<h4>Question – 6 (5-<em>pts</em>)</h4>

Run the algorithm for the Yale Face images (included in the lab material). Observe and discuss the results for different integration paths. Discuss how the images violate the assumptions of the shape-from-shading methods. Remember to include specific input images to illustrate your points. How the results would improve when the problematic images are all removed? Show the results in your report.

<h1><a name="_Toc8823"></a>2             Colour Spaces</h1>

In this part of the assignment, you will study the different colour spaces for image representations and experiment how to convert a given RGB image to a specific colour space.

<h3>RGB Colour Model (3-<em>pts</em>)</h3>

Why do we use RGB colour model as a basis of our digital cameras and photography? How does a standard digital camera capture the full RGB colour image?

<h3>Colour Space Conversion (10-<em>pts</em>)</h3>

Create a function to convert an RGB image into the following colour spaces by using the template code you are provided <strong>ConvertColourSpace.py </strong>and other sub-functions. Visualize the new image and its channels separately in the same figure. That is, for example, in the case of HSV colour space, you need to visualize the converted HSV image, and its Hue, Saturation and Value channels separately (4 images, 1 figure). Do not change the already given code.

<h3>Opponent Colour Space</h3>

<h3>Normalized RGB (rgb) Colour Space</h3>

<h3>HSV Colour Space</h3>

Convert the RGB image into HSV Colour Space. Use OpenCV’s built-in function <em>cv2.cvtColor(img, CV2.RGB2HSV)</em>.

<h3>YCbCr Colour Space</h3>

Convert the RGB image into YCbCr Colour Space. Use OpenCV’s built-in function <em>cv2.cvtColor(img, CV2.RGB2YCrCb)</em>. Note, you need to arrange the channels in <em>Y </em>, <em>C<sub>b </sub></em>and <em>C<sub>r </sub></em>order.

<h3>Grayscale</h3>

Convert the RGB image into grayscale by using 3 different methods mentioned in <a href="https://www.johndcook.com/blog/2009/08/24/algorithms-convert-color-grayscale/">https://www.johndcook.com/blog/2009/08/24/algorithms-convert-color-grayscale/ </a>In the end, check and report which method OpenCV uses for grayscale conversion, include it as well, and visualize all 4 in the same figure.

<h4>Colour Space Properties (5-<em>pts</em>)</h4>

Explain each of those 5 colour spaces and their properties. What are the benefits of using a different colour space other than RGB? Provide reasons for each of the above cases. You can include your observations from the visualizations.

<h4>More on Colour Spaces (2-<em>pts</em>)</h4>

Find one more colour space from the literature and simply explain its properties and give a use case.

<h1><a name="_Toc8824"></a>3             Intrinsic Image Decomposition</h1>

Intrinsic image decomposition is the process of separating an image into its formation components, such as reflectance (albedo) and shading (illumination).<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> Then, under the assumption of body (diffuse) reflection, linear sensor response and narrow band filters, the decomposition of the observed image <em>I</em>(<em>~x</em>) at position <em>~x </em>can be approximated as the element-wise product of its albedo <em>R</em>(<em>~x</em>) and shading <em>S</em>(<em>~x</em>) intrinsics:

<em>I</em>(<em>~x</em>) = <em>R</em>(<em>~x</em>) × <em>S</em>(<em>~x</em>)<em>.                                                </em>(1)

In this part of the assignment, you will experiment with intrinsic image components to perform one of the computational photography applications; material recolouring.

For the experiments, we will use images from a synthetic intrinsic image dataset.<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>

<h3>Other Intrinsic Components (4-<em>pts</em>)</h3>

What other components can an image be decomposed other than albedo and shading? Give an example and explain your reasoning.

<h3>Synthetic Images (2-<em>pts</em>)</h3>

If you check the literature, you will see that almost all the intrinsic image decomposition datasets are composed of synthetic images. What might be the reason for that?

<h3>Image Formation (4-<em>pts</em>)</h3>

Show that you can actually reconstruct the original <em>ball.png </em>image from its intrinsics using <em>ball albedo.png </em>and <em>ball shading.png</em>. In the end, your script should output a figure displaying the original image, its intrinsic images and the reconstructed one. Name your script as <strong>iid image formation.py</strong>.

<h3>Recoloring</h3>

Manipulating colours in photographs is an important problem with many applications in computer vision. Since the aim for recolouring algorithms is just to manipulate colours, better results can be obtained for such a task if the albedo image is available as it is independent of confounding illumination effects.

<h4>Recoloring (5-<em>pts</em>)</h4>

Assume that you are given the <em>ball.png </em>image and you have access to its intrinsic images <em>ball albedo.png </em>and <em>ball shading.png</em>.

<ol>

 <li>Find out the true material colour of the ball in RGB space (which is uniform in this case).</li>

 <li>Recolour the ball image with pure green (0,255,0). Display the original ball image and the recoloured version on the same figure. Name your script as <strong>py</strong>.</li>

 <li>Although you have recoloured the object with pure green, the reconstructed images do not seem to display those pure colors and thus the colour distributions over the object do not appear uniform. Explain the reason.</li>

</ol>

Note that this was a simple case where the image is synthetic, object centered and has only one colour, and you have access to its ground-truth intrinsic images. Real world scenarios require more than just replacing a single colour with another, not to mention the complexity of achieving a decent intrinsic image decomposition.

<h1><a name="_Toc8825"></a>4             Colour Constancy</h1>

Colour constancy is the ability to perceive colors of objects, invariant to the colour of the light source. The aim for colour constancy algorithms is first to estimate the illuminant of the light source, and then correct the image so that the corrected image appears to be taken under a canonical (white) light source. The task of the automatic white balance (AWB) is to do the same in digital cameras so that the images taken by a digital camera look as natural as possible.

In this part of the assignment, you will implement the most famous colour constancy algorithm; <em>Grey-World Algorithm</em>.

<h3>Grey-World Algorithm</h3>

The algorithm assumes that, under a white light source, the average colour in a scene should be achromatic (grey, [128, 128, 128]).

<h4>Grey-World (15-<em>pts</em>)</h4>

<ol>

 <li>Create a function to apply colour correction to an RGB image by using Grey-World algorithm. Display the original image and the colour corrected one on the same figure. Name your script as <strong>py</strong>. Use <em>awb.jpg </em>image to test your algorithm. In the end, you should see that the reddish colour cast on the image is removed and it looks more natural.</li>

</ol>

<em>Note: </em>You do not need to apply any pre or post processing steps. For the calculation or processing, you are not allowed to use any available code or any dedicated library function except <em>standard Numpy functions</em>.

<ol start="2">

 <li>Give an example case for Grey-World Algorithm on where it might fail. Remember to include your reasoning.</li>

 <li>Find out one more colour constancy algorithms from the literature and explain it briefly.</li>

</ol>

<h4>Hint</h4>

Check the von Kries model for the colour correction step.

<a href="#_ftnref1" name="_ftn1">[1]</a> H. G. Barrow and J. M. Tenenbaum. Recovering intrinsic scene characteristics from images. Computer Vision Systems, pages 3-26, 1978.

<a href="#_ftnref2" name="_ftn2">[2]</a> <a href="http://www.cic.uab.cat/Datasets/synthetic_intrinsic_image_dataset/">http://www.cic.uab.cat/Datasets/synthetic_intrinsic_image_dataset/</a>
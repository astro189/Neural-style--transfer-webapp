<h1>Neural Style Transfer</h1>

<h3>What is the neural style transfer?</h3>
<p>It is a generative deep learning model that is used for converting an image in to the style of another image. The first paper on neural style transfer (NST) was <b>"A Neural Algorithm of Artistic Style" by Leon Gatys et al</b>, published in 2015, in this paper was described the algorithm to implement the style transfer. The basic idea is to take the feature map outputs of certain layers of any CNN based architecture of three images called the content, style , generated images and the goal is for the generated image to attain a certain level of similarty with both the content and style image thus resulting in a stylized content image</p>

<p align="centre"><img src="https://github.com/astro189/Neural-style-transfer-webapp/blob/main/ReadMe_Images/Screenshot%202024-01-10%20031016.png" alt="Neural Style Transfer"></p>
<p align="center"><b>Neural Style Transfer</b></p>
<h3>Few links before we begin :)</h3>
<p>Try Neural Style Transfer for yourself (It might take some time to load) :<a href="https://neural-style-transfer-pxpl.onrender.com/">Neural Style Transfer</a></p>
<p>Repository containing the complete implementation of the notebook: <a href="https://github.com/astro189/Neural-Style-Transfer">Repository</a></p>

<h3>The CNN Model</h3>
<p>In the original implementation the VGG16 was used as the feature extractor. We would move forward with the same, next comes the question of which layers do we consider for the feature map outputs</p>
<p><b>Style Layers:</b> ['block1_conv2','block2_conv2','block3_conv3','block4_conv3','block5_conv3']</p>
<p><b>Content Layers:</b> ['block2_conv2']</p>
<p align="center"><img src="https://github.com/astro189/Neural-style-transfer-webapp/blob/main/ReadMe_Images/vgg16-architecture.width-1200.jpg" alt="VGG16", width=800px></p>
<p align="center"><b>VGG16 Architecture</b></p>
<h3>Content Loss</h3>
<p>Now we need a way to increase the similarity between the content and the generated image, to do so we can use a loss funcition which decreases as the similarity between the two increases. The loss function we use here is called the content loss and is essestially just the squared norm normalized over the channels, of the activation vectors of the two images from the the content layer.</p>
<p align="center"><img src="https://github.com/astro189/Neural-style-transfer-webapp/blob/main/ReadMe_Images/Screenshot%202024-01-10%20032043.png" alt="Content Loss" width=500px></p>
<p align="center"><b>Content Loss</b></p>
<p>Once we have a loss function we can optimize our weights using gradient descent to reduce this loss</p>

<h3>Style Loss</h3>
<p>The style loss is a bit more interesting, so for any artist when we refer to their style we usually refer to a set of common features that is primarily constant in their artworks. So, style of an image can basically be defined as a correlation between the different features of the artwork</p>

<p><b>For Eg:</b></p>
<p align="center"><img src="https://github.com/astro189/Neural-style-transfer-webapp/blob/main/ReadMe_Images/download.jpg" alt="Starry Night" width=300px></p>
<p align="center"><b>Starry Night by Van Gogh</b></p>
<p>In the above image we can see that the moons with the yellow colors are quite prominent and is a core part of the style. Also there is a strong correlation betwen the yellow color and the moon.</p>
<p>This is what we would like to capture in our final image to do so we use the Gram Matrix</p>

<h4>Gram Matrix</h4>
<p>The gram matrix is can simply be defined as a non mean centered covariance matrix. To get the gram matrix we just take the activation vector of our above mentioned style layers and perform the elementwise mutiplication with its transpose.</p>
<p align="center"><img src="https://github.com/astro189/Neural-style-transfer-webapp/blob/main/ReadMe_Images/Screenshot%202024-01-10%20031853.png" alt="Gram Matrix" width=500px></p>
<p align="center"><b>Gram Matrix</b></p>
<p>Physically the gram matrix represents the same idea as a covariance matrix where we try and get the covariance in between the different features of the feature map and helps us capture the style of the image</p>

<h4>Defining the Style Loss</h4>
<p>The gram matrix of our feature maps forms the style output we also do the same for the generated image,next its the same as for the content output, we try and increase the similarity between the generated image and the style image to do so we define the mean squared error loss for the gram matrix outputs</p>
<p align="center"><img src="https://github.com/astro189/Neural-style-transfer-webapp/blob/main/ReadMe_Images/Screenshot%202024-01-10%20032233.png" alt="Style Loss" width=500px></p>
<p align="center"><b>Style Loss</b></p>
<h3>Style and Content Weight</h3>
<p>To control the influence of the style and the content image on the final generated output we have the style_weight(alpha) and the content_weight(Beta). We multiply them with the respective losses to control the influence</p>
<p><b>Note:</b> Alpha and Beta are both constant and are not trained during optimization</p>

<h3>Total Variation Loss</h3>
<p>The net loss of the model is the total variation loss, its just the summation of the style and the content loss</p>
<p align="center"><img src="https://github.com/astro189/Neural-style-transfer-webapp/blob/main/ReadMe_Images/Screenshot%202024-01-10%20032432.png" alt="Style Loss" width=500px></p>
<p align="center"><b>Total Variation Loss</b></p>
<h3>Website Demo</h3>

https://github.com/astro189/Neural-style-transfer-webapp/assets/97799598/2101b72b-1fea-4287-849e-cb5c2e60b76f


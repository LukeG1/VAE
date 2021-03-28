# Variational Autoencoders
## Bringing In The Data
The first step is to create some kind of data loader to view your data. This is just an object that allows the training process to access the data it needs. For some data, Like MNIST, You may want to binarize the data, which just means all the pixel values are ether 0 or 1.
Data loaders can take images in from a folder of images, or the MNIST Dataset has a built in loader, or you can make your own.

## Network Structure
The chosen structure can exist at any level of complexity, even just a couple of linear layers would work. At its core it consists of an encoder and decoder, both as a sequence so that all their layers run in order in a concise way. 

### Encoder
The encoder will start with the image tensor (a flattened image), and compress it down to 2x the latent space.
This is because a variational autoencoder has what I refer to as pre-latent variables, which are basically just the mean and variance.
These 2 groups of information can later be combined into a latent space,

### Decoder
The decoder has the simple job of mirroring the encoder, with some small changes. The decoder doesn't start quite where the encoder leaves off, there is an intermediate step of combining the mean and variance. so the decoder starts with a layer the same size as the latent space (half what the encoder output).
The decoder also ends with a sigmoid layer to keep the values within reason.

### Methods

- **encode(x):**<br>This method takes a tensor image in and spits out the 2 things that make up the variational autoencoders pre-latent space
- **reparamaterize(mu, logvar):**<br>This takes the pre-latent variables and combines them into a tensor that is the latent representation of the input
- **encode_to_latent(x):**<br>This method combines the above methods to go directly from an image to a latent representation
-**decode(x):**<br>This takes the latent representation and turns it back into an image tensor 
- **forward(x):**<br>This method combines all the above methods to take an image completely through the process, It will most likely only be used in training  

## Loss Function
This is a weird loss function because it has to take into account multiple factors when determining loss.
The first element of this loss is the reconstruction loss, which is more like what a traditional autoencoder would use (how well does it recreate the image)
The second element is the KE divergence, which is a used to keep the statistics stuff in check, this ensures that what we train is actual being taught to be variational.
There is sometimes a third element of the loss function which when applied can lead to disentanglement, which is a good thing. This just encourages the model to train each dimension of the latent space to preform a specific task, meaning that if we change any one axis of an images latent space only one attribute of the picture would change 

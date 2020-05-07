# PokeType-Identifier
**By Kyle Cox, Conner Carlson, and Samuel Kratsas**

**Introduction**  
You open up your phone's photos, scroll through, and see a search bar suggesting items like "dog," "chair," or "car." Upon using the search bar, you learn that it's able to show you your pictures that include the searched item. This software works by using machine learning and a field called Convolutional Neural Networks to detect these items. These CNN models work at finding features (such as color, lines, shapes) that make the software believe it has found a particular item. Our smartphones make this task seem like a breeze, how hard is it actually to make a CNN Model for a corresponding set of images. In this project we will be creating a CNN of our own, and we will be testing it on Pokemon. Our model will be trained to  detect features such as the order and colors of pixels from an image of a Pokemon, and it will use these findings to make a near precise guess about the type of the Pokemon. Staying true to Pokemon, our project has evolved many times over the course of this study, so you will see how our model’s performance changes when used on a few different Pokemon image datasets.

**Illustration**

<a href="Map"><img src="https://firebasestorage.googleapis.com/v0/b/cardfli.appspot.com/o/Screen%20Shot%202020-05-06%20at%2011.54.15%20PM.png?alt=media&token=31253ee5-6032-46de-ad2c-f832aa0d82b9" align="center" height="260" width="390" ></a>

**Background and Related Work**  
  Due to our project being an image-based CNN, there are a plethora of examples we can choose from that are similar to our own project. For example, Google images has a tool that analyzes your images and finds out what your image is representing. The user is able to type in a characteristic, e.g., boat, and see all the photos that the Google image software decided include a boat. This is similar to our work due to both having image classification capabilities through the use of CNN's. Another related work is Method for Classification of Snowflakes Based on Images by a Multi-Angle Snowflake Camera Using Convolutional Neural Networks. In this article, the authors analyze snowflake images and attempt to classify them based on a variety of factors. The snowflakes go through an image classifier with a CNN, which includes having training/validation/test sets, layers, a pooling layer, a fully connected layer, and finally, a classified image. This is similar to our project due to our image classifier following a similar CNN outline, which contains all of the features listed above. Another similar work is Wave Monitoring Based on Improved Convolution Neural Network. In this article, an image of an ocean wave was put forth to assist in classification of the wave. This is akin to our software because we are taking an image of a Pokemon and using a CNN to classify the Pokemon.
 
 **Data Processing**  
  What had to be done with the data processing was a long, tedious process. Based on the original .csv file and image set that we gained from [Kaggle](https://www.kaggle.com/vishalsubbiah/pokemon-images-and-types), we had to go through all 809 Pokemon data entries and make sure the type was what the Pokemon looked most like. For instance, some Pokemon have a type of Water/Ice. We had to iterate through each Pokemon and decide which singular type fit it best and had to do this for every single entry. For example Noctowl, a literal owl, has the type Normal/Flying, so we needed to make sure he was given the classification of Flying. Also for what our specific model required, we needed to group each image into a directory based on type, then into Validation/Training/Test directories. To do this a [program](https://github.com/samuelKratsas/PokeType-Identifier/blob/master/sortEmAll.py) was created that uses the .csv file to take each image and put it into one of 18 directories based on its type from the .csv. Next, to split the 18 image sets into Validation/Training/Test directories an open-source Python library was used to randomly divide our images into Validation/Training/Test directories while still keeping them in their correct 18 type directories inside of those. In order to turn these images into data usable by our model the ImageDataGenerator class is called on to transform each image into a corresponding pixel array. 
  
  <a href="Noctowl"><img src="https://img.pokemondb.net/artwork/large/noctowl.jpg" align="center" height="190" width="95" ></a>
  
  Our 18 way classification image set though faced difficulty due to the fact the population of each class was so small, and to combat this we made two other image sets to run through our model. One is an image set with just two classes: Bug/Grass as Class 1 and Ground/Rock/Steel as Class 2. Lastly a 6 way classification image set with classes: Dark/Ghost, Ground/Rock/Steel, Grass/Bug, Normal/Fighting/Fairy, Water/Ice, Flying. These types were combined into these classes this way based on trends they seemed to share in their color and body design. For example Normal, Fighting, and Fairy all share lighter colors and humanoid appearances. 
  
  <a href="Mankey"><img src="https://static.pokemonpets.com/images/monsters-images-300-300/56-Mankey.png" align="center" height="160" width="150" ></a> <a href="Noctowl"><img src="https://img.pokemondb.net/artwork/large/miltank.jpg" align="center" height="160" width="150" ></a> <a href="Noctowl"><img src="https://img.pokemondb.net/artwork/large/wigglytuff.jpg" align="center" height="160" width="150" ></a>
  
**Architecture**  
  For our current model architecture, we continue to use Keras and Tensorflow to create a CNN such that it has three convolutional layers that consist of convolutions, activation functions, pooling layers, and drop out functions within each of these layers. These three layers extract out the features of the dataset and produce a result within the classifier layer. Furthermore, the classifier layer consists of a flatten function, dense with an activation function of ‘relu’, drop out functions, and another dense layer which fluctuates between 1 to 18 which is determined by the amount based on the number of Pokemon types that are being classified.This last dense layer is different based on the different data sets that are being run through the model which compare different Pokemon’s types. In total, the number of parameters that comes from these settings consist of 11,178,578. The reasoning for adding to the complexity in the architecture of the model consisted of taking a look at similar models that classified images and taking the similar principles that were used such as combining each layer with pooling layers and dropout functions. By having multiple convolutional layers rather than one, we hope to achieve better accuracy within our predictions of the dataset. Furthermore, within the actual testing sequence of the model, we implement an EarlyStopping function which stops the model from running too many iterations to prevent overfitting of the data.
  
   <a href="TableARC"><img src="https://firebasestorage.googleapis.com/v0/b/cardfli.appspot.com/o/Screen%20Shot%202020-05-07%20at%2012.46.41%20AM.png?alt=media&token=2c8c4060-c56c-4093-aeef-e1d5f5e8d132" align="center" height="520" width="310" ></a>

**Baseline Model**  
  For a baseline model, we thought it would be best to compare our model against one that guessed the type of the Pokemon at random. So we created a [simple model](https://github.com/samuelKratsas/PokeType-Identifier/blob/master/pokeBaseLine.py) that works by going through each Pokemon and randomly choosing a type from the 809 other pokemon in our set. This model works solely on randomness and the number of occurrences of each type. For example, type Water is 14% of the Pokemon set, so the model has a 14% chance of guessing Water for any given Pokemon. As one could guess the performance of this model is horrific; taking the average of 10 tries, the accuracies are displayed below. 
  
  <a href="TableBL"><img src="https://firebasestorage.googleapis.com/v0/b/cardfli.appspot.com/o/Screen%20Shot%202020-05-07%20at%2012.49.13%20AM.png?alt=media&token=69874fec-335a-425c-8e20-2ccd23a02317" align="center" height="150" width="200" ></a>

**Quantitive Results**  
  As stated before our model performed less than ideal on the standard 18 type image set, with around an 5% accuracy which in these tests is in fact worse than the baseline. Our solutions to this problem; the 2 class image set and the 6 class image set performed very well, averaging validation accuracies of 60% and 24% respectively. These are much better than their baselines: the 2 class set has a 20% accuracy increase and the 6 class set has a 50% increase.
    
  <a href="TableQuan"><img src="https://firebasestorage.googleapis.com/v0/b/cardfli.appspot.com/o/Screen%20Shot%202020-05-06%20at%206.48.47%20PM.png?alt=media&token=95c2edd9-60ce-4d64-95e6-b522da79bcb6" align="center" height="330" width="660" ></a>

**Qualitative Results**  
 There are several types in our model that seem to be predicted correctly most often such as Pokemon with Water, Grass, and Bug. However, our model gets easily confused by normal typing since it doesn’t really have a unique characteristic distinguishing it from types. There are also certain characteristics that overlap between different Pokemon that could cause our model to be inaccurate. Here are a couple of results from our data:
    
  <a href="TableQual"><img src="https://firebasestorage.googleapis.com/v0/b/cardfli.appspot.com/o/Screen%20Shot%202020-05-07%20at%201.12.15%20AM.png?alt=media&token=12645ba7-7478-42f9-896d-d6642e6d3209" align="center" height="300" width="640" ></a>

**Discussion**  
  In particular, to the results, we’ve noticed that our current model is only slightly outperforming our baseline model in scenarios that compare on fewer types and absolutely abysmal on comparing between all types. As one can see from the graphs above, our model is making some significant progress past the baseline on test sets with fewer types. Though these are small margins: our training accuracy is better than the randomly generated baseline’s accuracy, and our validation accuracy is better than both of them. We believe that the cause of this could potentially come from not having a big enough data set to train on even for the smaller testing scenarios. For example, we are trying to compare our 809 images into 18 different classes for our main test, which is the main contributor to having such a low accuracy rate in all of our tests so far with the model. 
  One consideration that was made to combat this issue was changing the classification of these images into two distinct classes rather than 18 in order to overcome this issue. For example, instead of classifying each image by their type, we could classify Pokemon on whether it is a water type or a normal type which we found significantly improved our model’s accuracy. We also tried different variants of this test and showed the results in the section above. Throughout this whole process, we found that having a bigger pool in the data samples helped significantly with training the model as well as only comparing between two different types. Despite only having a 60% accuracy rating, we feel that our model came a long way based on not having a large enough pool of images to train on as seen in our results.

**Ethical Considerations**  
  Some ethical considerations that would exist with our type of system that classifies images could consist of issues such as making incorrect decisions in a critical system that could cost people their lives or not being as transparent on why a particular decision was made on a certain type of image. Additionally, there can be ethical concerns that are social in nature such as having a bias towards the labels that are created to classify an image such as racial biases or what the system deems to be better or correct when it comes to classifying an image of a group of people. However, based on the limitations of the model and dataset only being restricted to making predictions against Pokemon images, most of these ethical considerations are not applicable unless the system was to extend past predicting images other than Pokemon.

**Project Difficulty**  
  Over the course of this project, we encountered multiple problems that needed to be rectified. To start, we had trouble working with 18 different classes of sizes between 30-80. This simply wasn’t big enough to accommodate 18 classes, which led us to our first problem which we ended up changing to 6 classes as we talked about above. Since we created our models to use these 18 classes however, we had to collectively change many aspects of the dataset in order to use our 6 class classifier. This was a tedious process which proved to be more difficult than we had originally intended. However, even this was not enough, so we decided to create another dataset of images to go down to a 2 class classifier. This gave us a decent boost in performance but was still not as much as we would have hoped. Overall, our team agrees that this project was definitely a challenge and we learned a lot from it. We agree that, given our similar-looking datasets and classes, our model performed exceptionally well considering it can be hard even for a human to classify a Pokemon’s type based on image alone.

**Conclusion**  
  Over the course of this study we learned alot about what works and what doesn’t work when designing a Machine Learning model and this was due to the challenges we faced. One point we would like to really drive home is that you should always try to have large class sizes for your model. As a general rule of thumb it seems to be the bigger class size the better, and as for the great datasets we use for our in class exercises: I saw one recently in Datacamp that had a class size of 2000, which is far more than our 800 total image set. I hope you remember this whenever you work with Machine Learning.
  
[**References**](https://github.com/samuelKratsas/PokeType-Identifier/blob/master/ourReferences.md)


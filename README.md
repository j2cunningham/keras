# keras

What happening in this custom generator:

 - A list of all the files in a location is passed to the class as well as a batch size
 - Each file becomes a batch and steps_per_epoch they are not needed.
 - A random index value is generated from 0 to the len of the list of files. The framework tracks with indexes have been used.
 - With a randomly generated index value, it is used to grab the entry in the file_list list corresponding to the index.
 - pd.read_csv on that specific file is called
 - The target variable is popped off the rest of the columns
 - If we wanted to do any transforms, we could do them here but I did my in Data Wrangler
 - The values of dependent (X) and independent (y) values is returned from the generator and feed to the keras model
 - The keras model learns from the data, updates it weights and then calls the next batch (file) until every file has been trained upon.
 - This completes one epoch
 
 
Notes: 
**batch_size** is ignored since it is the number of files
**on_epoch_end** is set to pass since the randomly generated index will just grab a file and don't need to keep track of a certain number of rows or entries in a list.
**steps_per_epoch** is not used because the number of files is the number of steps_per_epoch

### When would we use batch_size and steps_per_epoch?

Let's day we had 10,000 image files and wanted to train a model but all the images would not fit into memory.

Our workflow would be something like this:

Determine a batch size of some number of files we think will fit into memory, for example 32
steps_per_epoch would be 10,000/32 so that each epoch each file would be assigned to a batch
The number from above would be the length of the generator
The on_epoch_end in the class would need to implement a shuffle mechanism so that after each epoch, the contents of each batch change.
Note: There is a specific ImageDataGenerator class that has a flow_from_directory method. This class and method is great for not only pulling large files that won't fit in memory but also doing augmentation of the images along the way.

In a Keras custom generator, if you don't explicitly specify and track the index, it is managed implicitly by the keras.utils.Sequence class. The Sequence class automatically generates the indices based on the length of the data and the batch size

 



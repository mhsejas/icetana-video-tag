# **iCetana Video Tagger (AWS Rekognition)**
___________________________________________________________________________________________________________________________________________


## Introduction and Description

_NOTE: Please familiarise with what Amazon Rekognition is, it's a image and video processing API which uses deeplearning to identify items and objects of videos or images sent to it through AWS services, it returns labels and the confidences those labels apply to the video/image requested._

  This repository contains a juptyer notebook which parses JSON files as retrieved by the Amazon Rekognition API and selects the most unique labels above a particular confidence level (adjustable in the notebook itself), by counting the frequency of the labels of the particular directory. 
  
  The ideal way to accomplish this is to dump the AWS Rekognition json files into a particular directory, preferably in a `.txt` file format with the same file name. At the moment of this writing, supported file extensions are `.txt` and `.res` (resource file format). Additional file extensions can be added or removed by tweaking the notebook (at kernels 3 and 4). 
  
  The code will loop through the directory, open the json files, extract the labels above an user chosen confidence value, choose the most unique labels, extract the request ID, the date emitted and append into a data frame. The data frame will be sorted by date, descending from most recent request.
  
  JSON files with no extractable labels will be tagged with: __---NO LABELS DETECTED---__ and files inside the directory that cannot be opened because they are either not a JSON file, not properly formatted or their file extension not supported will also be logged. After looping through the entire directory, a data frame containing a descriptive tag, date of video, filename, and request ID will be outputted as a `.csv` file to the filepath set by the user. 
  
  The notebook is separated into five numbered, labelled kernels: 
  
  - `1.- INITIALISATION KERNEL` 
  - `2.- CUSTOMISATION KERNEL` 
  - `3.- LABEL PROCESSING KERNEL`
  - `4.- MAIN KERNEL`
  - `5.- DATA PROCESSING AND SUBMISSION KERNEL`
  
  Separated for ease of navigation and ease of use. If the only concern is to parse a folder full of JSON files your only changes should be made at kernel 2, the customisation kernel. 
  
  If there are any further code changes, or specific tweaking required, feel free to browse through the source code, it is very documented and simple to read.
  
 ## Parsing AWS Rekognition JSON files in a particular directory
 
 #### Background Information
 The notebook was formatted so it can be run sequentially.
 
 First step is to open the Video Tagger notebook.
 
 Run the first kernel, (`1.- INITIALISATION KERNEL`)
 
 Then head down to the following kernel. That will be the customization kernel and there are a few things to consider: 
 
 - You should familiarise yourself with the JSON format returned by AWS Rekognition, open in a text editor like Notepad one of the JSON files in your directory and observe that each video submitted has labels with confidence values as a percentage, corresponding to Rekognition's confidence that label applies to the video submitted. Hence the higher the confidence the higher chance the video contains it.
  
  However a problem arises that the API processes redudant labels with equal confidence values, e.g. Human, People, Person and hence rises the necessity of choosing the most descriptive, unique label which is why the algorithm discards the most popular labels. However there is  the compromise of lower confidence values giving wrong labels. Hence it is up to the user to customize those settings and that is exactly what this kernel allows you to do: 
  
#### Setting confidence threshold and confidence precision  

Setting the minimum accepted label confidence to be applied for video labelling is very simple, simply change the `confidence_threshold` variable to a floating point number between 0-100 (percentages). The default is set to 75% hence, you should find `confidence_threshold = 75.0`

If you have opened a couple JSON files from your directory, you might have perhaps not noticed that label confidences can differentiate between themselves with very small differences e.g. 0.000001. Hence rounding confidence values to a lower precision can improve video tagging by removing redudancies. By how much is up to the user and the number of decimal places allowed is set by the `conf_decimal_points` variable. The AWS default is 14 decimal places. 

#### Assigning directory file path and output path

Apart from the label filters, the final user required variables is the filepath of the directory containing the JSON files and the name and path of the csv file to be outputted. 

As default the program will prompt you for a path until a valid directory is entered, and the csv file will be labelled `results.csv` and outputted into the same directory. 

For quicker processing or when there is a single directory you can just hardcode the path by altering the following code in kernel 2: 
`directory_path = GetValidDirectoryPath()` to `directory_path = 'C:/Users/...'`.

The same can be done to decide where the resulting csv file will be made, simply changing the `output_file_name` variable to the full file path you want to output to. 

Run the rest of the notebook and you should be good to go!

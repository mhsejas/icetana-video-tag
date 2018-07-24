# **iCetana Video Tagger (AWS Rekognition)**
___________________________________________________________________________________________________________________________________________


## Introduction and Description

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
 
 The notebook was formatted so it can be run sequentially.
 
 First step is to open the Video Tagger notebook.
 
 Run the first kernel, i.e. `1.- INITIALISATION KERNEL`
 
 Then head down to the following kernel. That will be the customization kernel and there are a few things to consider: 
 
 - Firstly you should familiarise yourself with the JSON format returned by AWS Rekognition, open in a text editor 

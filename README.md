# INTRODUCTION
	Learning agreement is an agreement which is signed by the student during the initial days of joining where the student must opt for studying some of the bachelor’s degree subjects provided by the University in order to complete the missing 30 credits from student’s bachelor’s degree. Automation of this learning agreement process helps to avoid manual interactions as well as it saves time for both students and the staff in the administrative office. As a complete process of learning agreement from filling up to final approval takes a lot  of time because of the number of incoming students is more and the process is manual, this project’s aim is to reduce this time consumed in this manual process and to give an easy method for students to select subjects without any misunderstandings before submitting and for the staff to select the subjects which a student is allowed to take without much of a paper work before finally approving the learning agreement as everything is maintained online.
	In order to make this project success, it was important to understand the current scenario of learning agreement process along with selecting the tools which are efficient and reliable. The information regarding current workflow was collected from the staff of the administrative office who are responsible for handling the learning agreement process. Also, their suggestions on improvements in the current workflow helped in completing this project to automate the learning agreement in an efficient way.

# CURRENT STATE OF LEARNING AGREEMENT
	Many Universities in Germany expects a student who is going to start master course to have a minimum of 210 credits in the bachelor’s degree. In many cases, the student will be having only 180 credits in the bachelor’s degree thereby lacking the extra 30 credits to fulfil the entry requirement of the master course. This scenario is common for most of the students from Asia and they are provided with a conditional admit stating that the student must complete the extra 30 credits during the master course. The same method is being followed by Rhein-Waal University of Applied Sciences and the current process of learning agreement is completely manual and requires a lot of time and paperwork.

## CURRENT PROCESS

##	Student
a)	Visit Office: The process of learning agreement starts from filling out the ‘learning agreement form’ by the student by visiting the office. The student then must fill in the personal details and the desired list subjects in the form. The subjects must be selected from the set of allowed subjects provided by the staff.

b)	Master’s degree / Internship: If a student has already completed a master’s degree, then the student will have to submit the transcript of the master’s degree and fill only the personal details of the learning agreement form. Then the missing 30 credits will be completed by taking the credits from previous master’s degree. If case of a student who has completed an internship before joining the master’s degree, it is required that the student must submit a reference letter from the company where the internship was done along with the personal details in the learning agreement. Then the extra 30 credits will be waved off for the internship done by the student.

c)	Selecting Bachelor’s Subjects: Many students who don’t have a master’s degree or the internship certificate will have to choose a list of subjects provided by the University in order to complete the 30 credits. Student will have to submit the personal details along with the list of desired subjects during the submission of learning agreement from. Also, student has to submit a copy of the bachelor’s degree transcript during learning agreement submission.

##	Administration Office
a)	Validate and Approve: Office staff help the students while filling the learning agreement and once the form is completed they will cross verify the common subjects between subjects listed by the student and the subjects in the bachelor’s degree transcript in order to restrict the student from studying the same subject which the student has already completed in the bachelor’s degree. After verifying the details and the striking off the subjects which are not allowed to take by the student due to completion of similar subjects in bachelor’s degree, they pass the form to Head of Faculty and Head of Examination for reviewing. Once the review is done, they scan the document and mail the scanned copy to the student.

## DRAWBACKS
1.	Student must visit the office and wait for the availability of the staff.
2.	Staff should maintain and evaluate large number of applications as the incoming students are more.
3.	Process of evaluating the application to mailing the scanned copy to the student takes a lot of manual work and     	    consumes time also more prone to errors.
4.	Staff must go through the transcript of each student which may consist of 40 subjects which is a stressful work.
5.	If the student doesn’t receive the approved learning agreement, attending the desired courses during starting few 	  weeks is not possible.

## FUTURE STATE
Automating the Learning Agreement process will help students to know the courses they can take, thereby eliminating confusion. Our main aim is to reduce the human effort involved in signing learning agreement process between students and University.

## From student perspective: 
1.	If student is a new user, then register and login to the portal we have developed with his/her matriculation number 	    and mail Id.
2.	After login, students can create a learning agreement by uploading the transcript and selecting the desired subjects 	     to submit.
3.	If student is an existing user, students are allowed to only modify the learning agreement by choosing the desired             subjects and submit.

## From office staff perspective: 
1.	Login to the portal as admin.
2.	The uploaded transcript of the student and the subjects he / she can take is reflected. 
3.	Verify the requested subjects with his/her transcript for any duplication.
4.	If there are any duplication of subjects, reject those subjects and approve the learning agreement otherwise, approve 	      all subjects and the learning agreement.
5.	The approved learning agreement can be downloaded as PDF by students for their reference.

# WORKFLOW

![workflow](https://user-images.githubusercontent.com/52786804/64074522-a408eb80-ccac-11e9-87b6-a15fc3e62eff.png)

# ARCHITECTURE

![ALAP Architecture](https://user-images.githubusercontent.com/52786804/64074490-46749f00-ccac-11e9-8290-0a9c6c701437.png)

The application was developed using a 3-tier architecture where the layers were divided for abstraction, modularity and maintainability. The different layers are: 
1.	Client layer
2.	Service layer/Middleware
3.	Data layer

## Client layer
The client layer is responsible for rendering the interface to the user. The client interface was mainly developed to support desktop monitors and laptops. Client layer was developed using React.js – A JavaScript framework that runs on the node server. React js renders every user screen as a component. The client layer communicates with the Service layer by REST API. A set of endpoints that runs as a micro service is exposed on the server by URL and the client invokes the REST URL to fetch, create and modify data in the server. 

## Service layer
The service layer is designed as a micro service architecture where the components are divided into 3 micro services.

## ALAP API:
The ALAP API micro services are responsible for fetching, creating and updating user information and learning agreement related information from the database. This consists of a list of methods exposed as an API that can be invoked by the client. The micro service is hosted using ngrok which exposes the API endpoint to the internet which makes it easy to integrate against the client layer.

## Subject Extraction API:
A micro service developed in python, the subject extraction API is responsible for extracting the subject names amongst the string of words extracted from the student’s transcript. The micro service is exposed as API which can be accessed using an endpoint. It makes use of the Naïve Bayes classifier to classify the words and extract only the words which resembles the name of the subject and ignoring other details such as personal details of the student.

## Subject Comparison API:
The subject comparison is a python micro service which is used to compare the transcript subject with the subjects offered by the university. This was developed as a proof of concept and was not used for the actual implementation of the application.

## OCR Space API:
The OCR Space API is a third-party API which is used to perform Optical Character Recognition (OCR) in order to extract words from image. The free version of OCR Space API is used in the project implementation in which a REST endpoint is exposed, and the transcript is converted as a base64 string and uploaded to the endpoint. The API returns all the words extracted from the transcript in JSON format.

## Data Layer
The data layer consists of the database repository which stores and retrieves data based on the user request.  Microsoft SQL Server is used as the database in the project implementation. The data is stored and retrieved using stored procedure and queries. The database is connected to the ALAP API application which invokes the database using an Object Relational Model (ORM) called Dapper. The Dapper framework converts the request object into a database object and performs Create, Update, Read and Delete operations.

# MACHINE LEARNING ALGORITHM
The machine learning algorithm implemented in previous iteration was based on creating feature sets by using a feature extractor method. This method is search for a ‘feature’ in an input string and return the value as per the feature defined. During last project phase the features chosen were meagre in nature. To improve the process, more features were added to be considered as classifiers. The algorithm of previous implementation used feature to look for suffixes like ‘-ics’, ‘-ing’,’-ter’. The feature extractor method returned true if the input string consisted of these letters. A tuple of subject words tagged as ‘Subject’ and random words as ‘Not Subject’ is used to create a set of labelled words. Feature extractor method is used by classifier to create a feature set based on each labelled word having a feature to be likely either a ‘Subject’ or ‘Not Subject’. It worked based on the concept of ‘likelihood ratio’, which says that if a input string has a feature A, then it is more likely to be a ‘Subject’ as compared to a ‘Not Subject’.  Based on this likelihood ratio each new word is classified as a subject or random word. The classifier is then given the words extracted from PDF using OCR Space as input. This classifier was able to classify words as a subject with a certainty of around 68%. 

# PROCESS DESCRIPTION

![Description](https://user-images.githubusercontent.com/52786804/64074473-f564ab00-ccab-11e9-9513-4c5c25d9e9a7.png)



# RESULTS

![Register](https://user-images.githubusercontent.com/52786804/64074495-699f4e80-ccac-11e9-89f4-f2e4418eb19a.png)
![StudentDetails](https://user-images.githubusercontent.com/52786804/64074497-6a37e500-ccac-11e9-84b5-5505b83e6efe.png)
![Upload Transcript](https://user-images.githubusercontent.com/52786804/64074499-6a37e500-ccac-11e9-8ad1-0c5b0e42f295.png)
![Available Subjects](https://user-images.githubusercontent.com/52786804/64074995-b1c16f80-ccb2-11e9-8232-d7952164f7d6.png)
![Validate by Admin](https://user-images.githubusercontent.com/52786804/64074500-6a37e500-ccac-11e9-8bda-20e495c811f3.png)
![Subject Status](https://user-images.githubusercontent.com/52786804/64074498-6a37e500-ccac-11e9-80af-d547dd42a694.png)


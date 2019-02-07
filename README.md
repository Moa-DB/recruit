# For developers

This documentation is intended for developers who take over the development of this application. A description of use cases and requirements from the customer can be found in [this pdf](./application-description.pdf). This readme gives a general overview over the development and the structure of the application. For more detailed documentation, refer to the submodules of this repository and the source code documentation, which is also extensive.

## Structure
This system for handling applications is divided into three parts, each running on its own cloud instance.
* Application Client - Applicants can register, log in, and send their applications
* Recruiter Client - Recruiters can log in, review applications, and accept or reject applications.
* Server - The back end stack which provides a REST API that above clients can consume.

## Building & Deploying
* Source code collaboration is managed through git and GitHub.
* All three parts of the system are hosted on their own instance in the cloud at Heroku.
* Each part of the system is contained in a git submodule which can be worked on independently from the other parts.
* Continuous integration and continuous deployment is set up through Travis. Each part of the system has their own configuration for Travis contained in a .travis.yml. Travis is connected to GitHub and Heroku with a web hook. Travis is currently configured to build and deploy the master branch to Heroku when it is changed, whereas other branches, pull requests, etc. are built but not deployed.

_In short, this means that pushing code to the master branch (or merging code into it) will automatically build and deploy that part of the system which you are working on to Heroku._

## Recruiter Client
* A single-page web app in the React framework.

__Anders fyller på__


## Application Client
* A single-page web app in the React framework.

__Anders fyller på__

## Server

The back end is a Java Spring server with a MySQL database. The database is hosted via Heroku and ClearDB, see the Heroku account for more details. The server architecture is according to domain-driven design, which means that a normal call would pass this way through the layers:

__presentation__ -> __application__ -> __repository__ -> __domain__

A sample call to POST an application would thus go through these classes:

__ApplicationController__ -> __ApplicationService__ -> __ApplicationRepository__ -> __Application__

and return the application as JSON data to the client.
      
The server exposes a REST API:
### API List
These APIs are currently implemented on the server:

#### Permitted by all
* POST /perform\_login
* POST /register

#### Permitted by applicants
* POST /applications
* GET /competences

#### Permitted by recruiters
* POST /applications/filter
* PUT /applications/:id/accept
* PUT /applications/:id/reject
* PUT /applications/:id/unhandle
* GET /competences
* GET /statuses

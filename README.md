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

## Application Client & Recruiter Client
* The web apps has the overall same structure. They are both single-page web apps built with the React framework.
* The apps consists of three main parts. A __header__, page __content__ and a __footer__. The header and footer are always the same and the content in between is different depending on which page is loaded.
* The security and authentication process of the web app is handled server side via Spring Security. When a user is successfully authenticated we save this state in the frontend and the logged in user gets access to __restricted parts__. This is handled via __React Router__ where we have defined a PrivateRoute component. If a user tries to access a restricted part of the site without being authenticated it will simply be redirected to the path __/login__ again.
* We also need to think about __Cross-Origin Resource Sharing__ (CORS). Since the web applications are separated from the backend server and may be run on different domains. Browsers will restrict cross origin HTTP requests that are initiated from within scripts, in our case the fetch calls to the API. React has a good solution for this and all we have to do is to define a proxy to the server in __package.json__.

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

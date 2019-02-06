# Architectural Overview
The Architectural design of the final product is generally inspired by the microservice archictectural design philosophy, and also partially the client/server model. This is mostly due to the design constraints of the Electron framework, which in itself intends to act as a self-hosted client and server.

## Diagram
![Diagram](diagram.png)

### Architecture: Backend
The backend of the final product shall be constructed to act as a facilitator of pipeline executions and navigation. In order to fulfill these duties, the backend of the pipeline frontend will fulfill the following duties:


1. Act as a middle-man for communication between the pipeline and the frontend
2. Preserve the state of different pipeline modules


By acting as a facilitator, the front end of the final product would have a standardized method for performing pipeline functions, such as execution, and navigation between tabs. This will also also make the act of preserving state for navigation between pipeline modules, particularly back-navigaition, far more simple to implement in the frontend, as it would be stateless. This allows the backend to preserve state on behalf of the frontend.


### Architecture: Frontend (Pipeline Modules)
The front end of Primacy mainly consists of HTML, CSS and JavaScript for handling the way things are displayed.  We use Flexbox to manage our containers for each sub-module and tables for handling the steps within each of those.  Javascript also is used for handling the messages received from the backend.  The interprocess communication gets messages such as “NEW”, “EXECUTE” and “LOAD_MODULE”.  For example, when a user clicks on a submission button, the JavaScript attached to the button sends a message back to the back end containing the data from the entry fields on the page.

### Architecture: Pipeline
The pipeline is written in python and handles all of the more complicated computations.  Our program acts as the means to send parameters to the pipeline to be processed.  The pipeline actually executes the commands within the massive genome data sets and determines results to send back to our end.  

### Communication: IPC
Inter-Process Communication will be the primary medium of communication between the backend and frontend. This is mostly due to the fact that the fake client/server model imposed by electron would mean that the frontend and backend will exist of separate threads of an overreaching process. Due to this, IPC would be the most efficient method of communication. This method also allows for categorized information transmission, due to Electron's IPC system requiring that IPC messages define a 'channel' and 'body' for each message.

### Communication: JSON
JSON files are used as save state data.  In between each module, a JSON file is updated and modified to suit the changed inputs.  This allows traversal between each module, so each state can be reloaded if the user wants to go back to an earlier module.  JSONs are also sent to the API as parameters to the pipeline.  

### Communication: CMD Execution
Command execution will be the secondary method of communication from the backend to the pipeline, and can conceptually be regarded as a message to a particular pipeline module to execute, with arguments defined in an intermediary JSON file.

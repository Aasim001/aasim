
SessionRepository (Interface): 

com.infy.repository.Session Repository.java
This interface should extend the appropriate repository interface. 

• Add the following method to the interface. Use the method name approach to define the methods.
The method should accept batchid as a parameter and return the list of sessions as List.
The method should accept session startDate and endDate as a parameter 

#UnImplemented Code given : 

package com.infy.repository ;
public interface SessionRepository { 

}
Session Service (Interface): 

package com.infy.service;
import java.util.List; 

import com.infy.dto.BatchDTO; 

import com.infy.dto.SessionDTO; 

import com.infy.exception.SessionException; 

public interface SessionService { 

public Integer allocateSession(BatchDTO batchDTO) throws SessionException; 

public List<SessionDTO> viewBatchAllocation (Integer batchId) throws SessionException;
} 

Method Description: 

1. public Integer allocateSession (BatchDTO batchDTO): 

I • This method is used to allocate session(s) and educator(s) to the specific batch after validating the following conditions. 

• Retrieve existing sessions for the given batchld using appropriate repository method which returns the list of Session.


2. Iterate over the list of SessionDTO retrieved from the batchDTO received as method parameter. Use Streams to perform iteration. 

• Check if the session is already allocated to the given batch based on the course, if allocated then an object of Session Exception should be thrown with the message "SessionService ALREADY_ALLOCATED". 

• Check if any session is already allocated to the educator between the corresponding startDate and endDate. 

If an overlapping session is found, then throw a SessionException with the message "SessionService.EDUCATOR_UNAVAILABLE". 

• If the session startDate or endDate is after the batch endDate then throw a SessionException with the message "SessionService.INVALID_SESSION_DATE". 

• If the session endDate is before the session startDate, then throw a SessionException with the message "SessionService.INVALID_SESSION_END_DATE". 

• If all the above conditions are satisfied, allocate the session to the given batch by saving the Session object to the repository 

SessionServiceImpl class 

import com.infy.service; 
import java.util.List: 

@Service("sessionservice")
@Transactional 

public class SessionServiceImpl implements SessionService (
@Autowired
private SessionRepository sessionRepository: 

@Autowired
private ModelMapper modelMapper: 

@Override
public Integer allocateSession (BatchDTO batchDTO) throws SessionException {
      return null;
} 

@Override
public List<SessionDTO> viewBatchAllocation (Integer batchId) throws SessionException { 

return null;
}
}
API Class
com.infy.api.SessionAPI.java
This class should be implemented appropriately based on the class diagram and description given below: 

com.infy.api
sessionService: SessionService
restTemplate: Rest Template
environment: Environment
⚫allocateSession(batchDTO: BatchDTO): Response Entity
viewBatchAllocation(batchld: Integer): ResponseEntity 
⚫ viewBatchAllocationFallback(batchld: Integer, throwable: Throwable): ResponseEntity




SessionAPI Implementation 

com.infy.api
sessionService: SessionService
rest Template: Rest Template
environment: Environment
allocateSession(batchDTO: BatchDTO):Response Entity 

viewBatchAllocation(batchid: Integer):ResponseEntity 

viewBatchAllocationFallback(batchid Integer, throwable: Throwable): ResponseEntity 
This class should be made as REST controller class.
All the methods in this class should have
/session as base URL.
Instance of SessionService, Environment and Rest Template classes should be injected.
I
(Note: Use the load balanced RestTemplate for Service Discovery of BatchMS inside the methods.) 

Method Description:
public Response Entity allocateSession (BatchDTO batchDTO):
• This method is used to allocate sessions and educators to the given batch.
• This method should accept POST request with URI as/allocate.
• The BatchDTO should be validated
Invoke the appropriate RestTemplate class method of the BatchMS by passing the batchid from BatchDTO object received as method parameter which will in turn returns the BatchDTO object.
• Set the endDate of the batchDTO to be the same as the endDate of the BatchDTO object retrieved from the web client call.
• Invoke the appropriate method of SessionService to attempt allocating sessions based on the information within the batchDTO.
• If the session allocation is successful then it will return the batchld.e 
• Retrieve the message associated with property SessionAPI.ALLOCATION_SUCCESS and append it to batchld in the following format: batchid
• Return a Response Entity object with the success message and an HTTP status code of CREATED.
public Response Entity viewBatchAllocation (Integer batchid)
This method is used to view the allocation of sessions and educators for the specified batchld.
• It accepts GET request with URI as/view-batch/allocation/(batchid).
• Invoke the appropriate Rest Template class method by passing batchld as an argument to retrieve the BatchDTO object. 

Invoke the appropriate service class method by passing batchld which will return the list of sessionDTO, list, allocated for the batchid.
• Iterate over the retrieved list to set the courseDTO for each sessionDTO object. The courseDTO can be retrieved by making Rest Template call to the Alt defined to fetch the Course based on courseld in the BatchMS. 
• Set the list of sessionDTO to the batchDTO.
• Return a Response Entity object with the batchDTO and an HTTP status code of OK.
Write a fallback method with method name viewBatchAllocationFallback if the above viewBatchAllocation method fails.
• Inside fallback method set the parameters of BatchDTO as below:
"batchId": null,
"batchName": "In Fallback Method",
"startDate": null,
"endDate": null,
"batchOwner": null,
"sessionD10": null,



Code: 

package com.infy.api;
import org.springframework.core.env.Environment; 

import org.springframework.http.ResponseEntity; 

import org.springframework.web.bind.annotation.PathVariable; 

import org.springframework.web.client.RestTemplate; 

import com.infy.dto.BatchDTO;
import com.infy.exception. SessionException;
import com.infy.service.SessionService; 

public class SessionAPI { 

private SessionService sessionService;
private RestTemplate restTemplate;
private Environment environment; 

public ResponseEntity<String> allocateSession(BatchDTO batchDTO) throws SessionException{
return null;
}
public ResponseEntity <BatchDTO> viewbatchAllocation (@PathVariable Integer batchId) throws SessionException {
return null;
}
}





_---------_---------_---------

API Class
com.infy.api.SessionAPI.java
This class should be implemented appropriately based on the class diagram and description given below:
SessionAPI
com.infy.api
sessionService: SessionService
restTemplate: Rest Template
environment: Environment
⚫allocateSession(batchDTO: BatchDTO): Response Entity
viewBatchAllocation(batchld: Integer): ResponseEntity 
⚫ viewBatchAllocationFallback(batchld: Integer, throwable: Throwable): ResponseEntity
SessionAPI
com.infy.api
sessionService: SessionService
rest Template: Rest Template
environment: Environment
allocateSession(batchDTO: BatchDTO): Response Entity
viewBatchAllocation(batchid: Integer): ResponseEntity 
viewBatchAllocationFallback(batchid Integer, throwable: Throwable): ResponseEntity 
This class should be made as REST controller class.
All the methods in this class should have /session as base URL.
Instance of SessionService, Environment and Rest Template classes should be injected.
I (Note: Use the load balanced RestTemplate for Service Discovery of BatchMS inside the methods.)
Method Description:
public Response Entity allocateSession (BatchDTO batchDTO):
O This method is used to allocate sessions and educators to the given batch.
o This method should accept POST request with URI as/allocate.
The BatchDTO should be validated
Invoke the appropriate RestTemplate class method of the BatchMS by passing the batchid from BatchDTO object received as method parameter which will in turn returns the BatchDTO object.
Set the endDate of the batchDTO to be the same as the endDate of the BatchDTO object retrieved from the web client call.
• Invoke the appropriate method of SessionService to attempt allocating sessions based on the information within the batchDTO.
If the session allocation is successful then it will return the batchld.
e Retrieve the message associated with property SessionAPI.ALLOCATION_SUCCESS and append it to batchld in the following format:
: batchid
o Return a Response Entity object with the success message and an HTTP status code of CREATED.
public Response Entity viewBatchAllocation (Integer batchid)
This method is used to view the allocation of sessions and educators for the specified batchld.
o It accepts GET request with URI as/view-batch/allocation/(batchid).
o Invoke the appropriate Rest Template class method by passing batchld as an argument to retrieve the BatchDTO object.
Invoke the appropriate service class method by passing batchld which will return the list of sessionDTO, list, allocated for the batchid.
o Iterate over the retrieved list to set the courseDTO for each sessionDTO object. The courseDTO can be retrieved by making Rest Template call to the Alt defined to fetch the Course based on courseld in the BatchMS. I
Set the list of sessionDTO to the batchDTO.
o Return a Response Entity object with the batchDTO and an HTTP status code of OK.
Write a fallback method with method name viewBatchAllocationFallback if the above viewBatchAllocation method fails. Inside fallback method set the parameters of BatchDTO as below:
"batchId": null,
"batchName": "In Fallback Method",
"startDate": null,
"endDate": null
"batchOwner": null,
"sessionD10": null
As per this command give the code
import java ut Αεταλλ
23 Bhequrathapsing value
24 @Validated
23 pubite clase SessiOAPTI
200
BAutowired
private SessionServion rastoneretz
BAutows.red
300 private BessTemplate reasTemplate/
private favironmens environmen
Pintmagging (value/allocate)
public ResponseEntasychtring allocatecasion (BValid PathVariable BarchDTO batchDTO) throws SeasionExceptio
return Ma
public BesponseEntity Batchiro viewbatchAllocation (@PathVariable Integer batchid) throws SessionException t
return null

package com.infy.api;

import com.infy.dto.BatchDTO;
import com.infy.dto.SessionDTO;
import com.infy.service.SessionService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
@RequestMapping("/session")
@Validated
public class SessionAPI {

    @Autowired
    private SessionService sessionService;

    @Autowired
    private RestTemplate restTemplate;

    @Autowired
    private Environment environment;

    @PostMapping("/allocate")
    public ResponseEntity<String> allocateSession(@RequestBody BatchDTO batchDTO) {
        // Validate BatchDTO
        if (batchDTO == null || batchDTO.getBatchId() == null) {
            return new ResponseEntity<>("Invalid BatchDTO", HttpStatus.BAD_REQUEST);
        }

        // Retrieve BatchDTO from BatchMS
        String batchMsUrl = "http://BatchMS/batch/" + batchDTO.getBatchId();
        BatchDTO retrievedBatch = restTemplate.getForObject(batchMsUrl, BatchDTO.class);

        if (retrievedBatch == null) {
            return new ResponseEntity<>("Batch not found", HttpStatus.NOT_FOUND);
        }

        // Set the endDate of batchDTO to retrieved batch's endDate
        batchDTO.setEndDate(retrievedBatch.getEndDate());

        // Call service layer to allocate sessions
        Integer allocatedBatchId = sessionService.allocateSessions(batchDTO);

        if (allocatedBatchId != null) {
            String successMessage = environment.getProperty("SessionAPI.ALLOCATION_SUCCESS") + ": " + allocatedBatchId;
            return new ResponseEntity<>(successMessage, HttpStatus.CREATED);
        } else {
            return new ResponseEntity<>("Session allocation failed", HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }

    @GetMapping("/view-batch/allocation/{batchid}")
    public ResponseEntity<BatchDTO> viewBatchAllocation(@PathVariable Integer batchid) {
        // Retrieve BatchDTO from BatchMS
        String batchMsUrl = "http://BatchMS/batch/" + batchid;
        BatchDTO batchDTO = restTemplate.getForObject(batchMsUrl, BatchDTO.class);

        if (batchDTO == null) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }

        // Retrieve session details for the batch
        List<SessionDTO> sessionList = sessionService.getSessionsForBatch(batchid);

        if (sessionList != null) {
            for (SessionDTO session : sessionList) {
                // Fetch CourseDTO using courseId from BatchMS
                String courseUrl = "http://BatchMS/course/" + session.getCourseId();
                session.setCourseDTO(restTemplate.getForObject(courseUrl, SessionDTO.CourseDTO.class));
            }
        }

        batchDTO.setSessionDTO(sessionList);
        return new ResponseEntity<>(batchDTO, HttpStatus.OK);
    }

    @GetMapping("/view-batch/allocation-fallback/{batchid}")
    public ResponseEntity<BatchDTO> viewBatchAllocationFallback(@PathVariable Integer batchid, Throwable throwable) {
        BatchDTO fallbackBatch = new BatchDTO();
        fallbackBatch.setBatchId(null);
        fallbackBatch.setBatchName("In Fallback Method");
        fallbackBatch.setStartDate(null);
        fallbackBatch.setEndDate(null);
        fallbackBatch.setBatchOwner(null);
        fallbackBatch.setSessionDTO(null);

        return new ResponseEntity<>(fallbackBatch, HttpStatus.SERVICE_UNAVAILABLE);
    }
}





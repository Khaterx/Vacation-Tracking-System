
### Step 1: Employee logs into portal
```pseudocode
function login(employee)
	if isAuthenticated(emplyeeId):
		if asManager(employee):
			Display "list of pending requests"
		else:
		return employeeBalance
		END
	else:
		Display "Access Denied"
	END
```
### Step 2: Employee creates a new request
```pseudocode
function createVacationRequest(startDate,endDate,requestType)
		displayLeavesBalance();
	  if isEmployeeValid(employee) and if isValidDateRange(start_date, end_date)
		   and if isValidCategory(requestType):
			 return "Submit or Cancel" options
			 if isSubmit:
				vacationRequest = CREATE new VacationRequest WITH status = "Pending"
				updateLeavesBalance(vacationRequest);
				sendEmailToManagerAndAddToPendingList(vacationRequest);
				return "Submit Successfully"
			else:
				return to homePage
			END
		else
		   display "Invalid input data"
	  END
```

### Step 3: Manager Responds to Vacation Request
```pseudocode
function vacationResponse(manager, vacationRequest, isApproved, message) 
    if (isManagerAuthorized(manager, vacationRequest)) 
        if (isApproved) 
            updateRequestStatusAsApproved(vacationRequest);
         else 
            updateRequestStatusAsRejected(vacationRequest);
            attachRejectionMessage(vacationRequest, message);
        END
        sendEmailToEmployee(vacationRequest);
     else
        return "Manager not authorized to approve/reject this request.";
    END

```


### Step 4:  Withdraw a Pending Vacation Request:
```pseudocode
function withdrawVacationRequest(employee)
	if isEmployee(employee):
	  displayVacationRequestSummary(employee);
	  vacationRequest = selectPendingRequestToWithdraw();
		if(vacationRequest != null && requestStatusIsPending(vacationRequest)):
			if(confirmWithdrawRequest(vacationRequest)):
				removeFromManagerPendingList(vacationRequest);
				 updateRequestStatusToWithdrawn(vacationRequest);
				 sendEmailToManager(vacationRequest);
				 return "Vacation request withdrawn.";
			else:
				 return "Withdrawal canceled by employee.";
			 END
		else:
			return "No pending request selected.";
		END	
	else:
	  return "Invalid employee.";
	END
```

### Step 5:  Cancel an Approved Vacation Request:
```pseudocode
function cancelApprovedRequest(employee)
	if isEmployee(employee):
	  displayVacationRequestSummary(employee);
	  vacationRequest = selectApprovedRequestToCancel();
	   if (vacationRequestInFuture(vacationRequest) ||acationRequestInRecentPast(vacationRequest)):
		    if (vacationRequestInRecentPast(vacationRequest)):
				 getShortCancellationExplanation();
			 confirmCancellation(vacationRequest);
				 if (employeeApprovesCancellation()):
					 updateRequestStatusToCanceled(vacationRequest);
					 notifyManagerByEmail(vacationRequest);
					 returnVacationTimeToEmployee(employee, vacationRequest);
					 return "Request canceled successfully.";
				 else:
					 return "Cancellation aborted."
				END
		else:
			return "Request cannot be canceled." 
		END
	else:
		return "Invalid employee.";
	END
```
---
### 📃 Pseudocode:
```pseudocode
START

// Step 1: Employee logs into portal
IF employee IS authenticated THEN
    SHOW VTS homepage
    FETCH vacation requests history (last 6 months to next 18 months)
    SHOW vacation balance
ELSE
    DISPLAY "Access Denied"
    END

// Step 2: Employee creates a new request
PROMPT employee TO select vacation category WITH positive balance

IF category IS valid THEN
    PROMPT employee TO select start and end dates, hours per day
    PROMPT employee TO enter title and description

    // Step 3: Validate data
    IF input IS valid THEN
        CREATE new VacationRequest WITH status = "Pending"

        // Step 4: Check if manager approval is required
        IF manager_approval_required THEN
            SEND email TO manager WITH approval link
        ELSE
            SET VacationRequest.status = "Approved"
        ENDIF

        SAVE VacationRequest TO database
        DISPLAY "Request submitted successfully"
    ELSE
        DISPLAY validation errors
        ALLOW employee TO edit or cancel
    ENDIF
ELSE
    DISPLAY "Invalid category or no balance"
ENDIF

END

```


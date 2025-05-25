```
FUNCTION submitVacationRequest(employeeID)
    // PRECONDITION: Employee must be authenticated
    IF NOT isAuthenticated(employeeID) THEN
        RETURN "Error: Authentication required"
    ENDIF

    // MAIN FLOW
    // Step 1-2: Display dashboard
    pastRequests = getRequests(employeeID, timeRange: "6 months past")
    futureRequests = getRequests(employeeID, timeRange: "18 months future")
    balances = getVacationBalances(employeeID)
    displayDashboard(pastRequests, futureRequests, balances)

    // Step 3-4: Collect new request data
    newRequest = {
        category: promptForCategory(balances),
        dates: promptForDates(),
        hoursPerDay: promptForHours(),
        title: promptForTitle(),
        description: promptForDescription()
    }

    // Step 5-6: Validate request
    validationResult = validateRequest(newRequest, employeeID)
    
    // Step 7: Process based on validation
    IF validationResult.isValid THEN
        newRequest.status = "Pending Approval"
        storeRequest(newRequest)
        
        IF requiresApproval(newRequest, employeeID) THEN
            manager = getManager(employeeID)
            sendApprovalRequest(manager, newRequest)
            notification = "Request submitted for approval"
        ELSE
            newRequest.status = "Approved"
            updateRequestStatus(newRequest)
            updateBalances(employeeID, newRequest)
            notification = "Request automatically approved"
        ENDIF
        
        sendNotification(employeeID, notification)
        RETURN "Success: " + notification
    ELSE
        displayErrors(validationResult.errors)
        RETURN "Error: Validation failed"
    ENDIF
END FUNCTION

// Supporting functions
FUNCTION validateRequest(request, employeeID)
    errors = []
    
    // Check required fields
    IF request.category is empty THEN
        errors.add("Category required")
    ENDIF
    
    // Check date validity
    IF request.dates.start > request.dates.end THEN
        errors.add("Invalid date range")
    ENDIF
    
    // Check business rules
    IF exceedsMaxDuration(request) THEN
        errors.add("Exceeds maximum allowed duration")
    ENDIF
    
    RETURN {
        isValid: (errors.length == 0),
        errors: errors
    }
END FUNCTION
```
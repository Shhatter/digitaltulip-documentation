# Defect Management in Jira

# Step by Step guide

1. Issues for testing are those included in the Testing swim lane of the scrum board in JIRA

![alt text](/Images/jiratest.png)

2. If a test has passed
        Record the results as a comment, attach screenshot or other attachment if appropriate
        If further testing is required, e.g. manual testing has been done but automated testing has not been completed, then leave the status as 'Testing'
        Otherwise, change the status to 'Done'
3. If a test has failed
        Record the results as a comment
        Create a sub-task, pre-fix the title with "Defect:"
        The sub-task is added to the To Do swim lane on the scrum board


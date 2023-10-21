#### Jobs
- In case a job experiences an InterruptedExecution, it's rescheduled somewhere else
- In case a job completes and returns a Non-zero Exit Code, you need to define a restartPolicy for the same
- When a job cpmplete successfully and returns a Zero exit code, Its status is set to completed - The Job object remains - The pods are also not deleted
- 
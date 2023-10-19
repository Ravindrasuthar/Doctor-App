# Project Name
Doctor App

# Frameworks and Language Used
**Spring Boot** 3.1.4
**Java** 17
**Maven** 3.8.1

# Data Flow


_**Controller:**_ The controller has endpoints for add a doctor, get a doctor by id, get all doctor, get all patients, get all patients by bloodgroup, patient signup, patient signin, patient signout, add appointment, delete appointment and get doctor by qualification or specialization.

The @PostMapping annotation is used for the doctor, patient/signup, patient/signIn, patient/appointment/schedule,  endpoint to handle HTTP POST requests with a JSON request body containing a required object. 
```java
// In AdminController

  @PostMapping("doctor")
    public String addDoctor(@RequestBody Doctor newDoctor)
    {
        return doctorService.addDoctor(newDoctor);
    }
```
```java
// In PatientController

@PostMapping("patient/signup")
    public String patientSignUp(@Valid @RequestBody Patient patient)
    {
        return patientService.patientSignUp(patient);
    }

    //sign in
@PostMapping("patient/signIn")
    public String patientSignIn(@RequestBody SignInInputDto signInInput)
    {
        return patientService.patientSignIn(signInInput);
    }

 @PostMapping("patient/appointment/schedule")
    public String scheduleAppointment(@RequestBody ScheduleAppointmentDTO scheduleAppointmentDTO)
    {
        return appointmentService.scheduleAppointment(scheduleAppointmentDTO.getAuthInfo(),scheduleAppointmentDTO.getAppointment());
    }
```
The @GetMapping annotation is used for the patients, patients/bloodGroup/{bloodGroup}, doctors, doctor/{id} and doctors/qualification/{qual}/or/specialization/{spec} endpoints to handle HTTP GET requests with and without a path variable for the id, with path variable as bloodgroup, with requestbody as authInfo and path variable as qual and spec . 
The @PathVariable annotation is used to extract the required ID or any member variable from the request URL and pass it to the curresponding method.
```java
// Inside AdminController

 @GetMapping("patients")
    public List<Patient> getAllPatients()
    {
        return patientService.getAllPatients();
    }
 @GetMapping("patients/bloodGroup/{bloodGroup}")
    public List<Patient> getAllPatientsByBloodGroup(@PathVariable BloopGroup bloodGroup)
    {
        return patientService.getAllPatientsByBloodGroup(bloodGroup);
    }
```
```java
// Inside DoctorController

 @GetMapping("doctors")
    public List<Doctor> getAllDoctors(@Valid @RequestBody AuthenticationInputDto authInfo)
    {
        return doctorService.getAllDoctors(authInfo);
    }


    @GetMapping("doctor/{id}")
    public Doctor getDoctorById(@PathVariable Integer id)
    {
        return doctorService.getDoctorById(id);
    }
```
```java
//Inside PatientController

@GetMapping("doctors/qualification/{qual}/or/specialization/{spec}")
    public List<Doctor> getDoctorsByQualificationOrSpec(@RequestBody AuthenticationInputDto authInfo,@PathVariable Qualification qual,@PathVariable Specialization spec)
    {
        return doctorService.getDoctorsByQualificationOrSpec(authInfo,qual,spec);
    }
```

The @DeleteMapping annotation is used for the patient/signOut and patient/appointment/{appointmentId}/cancel endpoint to handle HTTP DELETE requests with a requestbody for the AuthenticationInputDto and path variable for the appointmentId.
```java
// Inside PatientController

 @DeleteMapping("patient/signOut")
    public String patientSignOut(@RequestBody AuthenticationInputDto authInfo)
    {
        return patientService.patientSignOut(authInfo);
    }
@DeleteMapping("patient/appointment/{appointmentId}/cancel")
    public String cancelAppointment(@RequestBody AuthenticationInputDto authInfo, @PathVariable Integer appointmentId)
    {
        return appointmentService.cancelAppointment(authInfo,appointmentId);
    }
```
The controller class also has an autowired all the required Service interface to handle business logic for the Doctor App.

This implementation demonstrates a basic setup for a REST API controller in Spring Boot, but it can be expanded upon and customized based on specific requirements for the Doctor App.


_**Services**:_ The services layer contains the business logic of the application. It receives requests from the controller, performs necessary computations or data manipulations, and interacts with the repository layer to access data.
```java
```

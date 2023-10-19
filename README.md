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
@Service
public class AppointmentService {

    @Autowired
    IAppointmentRepo appointmentRepo;
    @Autowired
    IPatientRepo patientRepo;
    @Autowired
    IDoctorRepo doctorRepo;
    @Autowired
    PTokenService pTokenService;
```

```java
@Service
public class PatientService {

    @Autowired
    IPatientRepo patientRepo;
    @Autowired
    PTokenService pTokenService;
```

```java
@Service
public class DoctorService {

    @Autowired
    IDoctorRepo doctorRepo;
    @Autowired
    PTokenService pTokenService;
```

```java
@Service
public class PTokenService {
    
    @Autowired
    IPTokenRepo ipTokenRepo;

    public void createToken(PatientAuthenticationToken token) {
        ipTokenRepo.save(token);
    }
```

_**Repository:**_ The repository layer is responsible for interacting with the database. It uses Spring Data JPA to perform CRUD (create, read, update, delete) operations on entities.

In the application.properties all the text required for connection with MySQL database are written.
```java
spring.datasource.url=jdbc:mysql://localhost:3306/docAppointmentDB_fs_11
spring.datasource.username=root
spring.datasource.password=ina728dg
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update

spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.use_sql_comments=true
spring.jpa.properties.hibernate.format_sql=true

```
I have used EmailService class to send the token to the patient email when He/She will login using email and password.
```java
public class EmailService {

    private static final String EMAIL_USERNAME = "mainakgh1@gmail.com";
    private static final String EMAIL_PASSWORD = "eige yneu huem bhzz";
```
I have used PasswordEncryptor class to encrypt the password.
```java
public class PasswordEncryptor {

    public static String encrypt(String unHashedPassword) throws NoSuchAlgorithmException {
        MessageDigest md5=MessageDigest.getInstance("MD5");

        md5.update(unHashedPassword.getBytes());
        byte[]digested=md5.digest();
        return DatatypeConverter.printHexBinary(digested);

    }
}
```

# Database Structure Used
I have used MySQL as DataBase

# Project Summary
In this project i have created different endpoints, custom finders, Validation, @OneToOne, @ManyToOne and @OneToMany mapping.

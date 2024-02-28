**Patient.java**
<p><code>
    @Entity
    @Table(name="patient")
    public class Patient{
          @Id
          @GeneratedValue(strategy=GenerationType.IDENTITY)
          private Integer patientId;
    }
</code></p>

**PatientDTO.java**
<p><code>
    private Integer patientId; 
	
	@NotNull(message="{patient.name.notpresent}")
	@Pattern(regexp = "[a-zA-Z]+([\sa-zA-Z])*", message="[patient.name.invalid}")
	private String patientName;
	
 	@NotNull(message="{patient.gender.notpresent}")
	@Pattern(regexp = "(Male|Female|Others)", message="{patient.gender.invalid}")
	private String gender;
 
	private LocalDate date Of Birth;
 
	@NotNull(message="{patient.admissiondate.notpresent}") 
    @FutureOrPresent(message="{patient.admissiondate.invalid}")
	private LocalDate admissionDate;
 
	@NotNull(message="{patient.diagnosis.notpresent}")
	@Patternregexp = "[a-zA-Z0-9\s]+", message="{patient.diagnosis.invalid}") 
    private String diagnosis;
</code></p>

**PatientValidator.java**

<p><code>
	public void validatePatient(--------)
	{
		if(!isValiddateOfBirth(patientDTo.getDateOfBirth())){
			throw new PatientAdmissionException("PatientValidator.INVALID_DOB");
		}
	}
	public static Boolean isValidDateOfBirth(-------------)
	{
		if(dateOfBirth.isAfter(LocalDate.now())){
			return false;
		}
		if(LocalDate.now().getYear()-dateOfBirth.getYear()>100){
			return false;
		}
		return true;
	}
</code></p>

**PatientRepository.java**
<p><code>
	public interface PatientRepository extends CrudRepository<*patient, Integer>{
		List<*Patient> findByDiagnosis(String d);
	}
</code></p>

**PatientServiceImpl.java**
<p><code>
	@Service(value="patientService")
	@Transactional
	public class PatientServiceImpl implements PatientService {
		@Autowired
		private PatientRepository patientRepository;
		@Override
		public List<*PatientDTO> getListOfPatients(-----------){
			List<*Patient> patient=patientRepository.findByDiagnosis(diagnosis);
			if(patient.isEmpty()|| patient.size()==0){
				throw new PatientAdmissionException("PatientService.PATIENT_UNAVAILABLE");
			}
			List<*PatientDTO> dtos=new ArrayList<*PatientDTO>();
			patient.stream().forEach(x->dtos.add(new PatientDTO().prepareDTO(x)));
			dtos.sort((x1,x2)->x1.getAdmissionDate().compareTo(x2.getAdmissionDate()));
			return dtos;
		}

  		@override
    		public PatientDTO admitPatient(PatientDTO) 
      		{
			PatientValidator.validatePatient(patientDTO);
   			Patient patient =PatientDTO.prepareEntity(patientDTO);
      			patient=patientRepository.save(patient);
	 		patientDTO.setPatientId(patient.getPatientId());
    			return patientDTO;
    		}
      }
</code></p>

**PatientAPI.java**
<p><code>
	@RestController
	@RequestMapping(value="/api")
	public class PatientAPI{
		@Autowired
		private PatientService patientService;

  		@GetMapping(value="/patients/{diagnosis}")
  		public ResponseEntity<*List<*PatientDTO>> getPatientsByDiagnosis(@PathVariable @Pattern(regexp="[a-zA-Z0-9\s]+", message="{patient.diagnosis.invalid}") String diagnosis) throws PatientAdmissionException
    		{
      			List<*PatientDTO> patientDTOs =patientService.getListOfPatients(diagnosis);
	 		return new ResponseEntity<List<PatientDTO>>(patientDTOs,HttpsStatuse.OK);
    		}
      
    		@PostMapping(value="/patients")
      		public ResponseEntity<*List<*PatientDTO>> admiPatient(@RequestBody PatientDTO patientDTO)
		{
 			PatientDTO p=patientService.admiPatient(patientDTO);
    			return new ResponseEntity<PatientDTO>(p,HttpStatus.CREATED);
       		}
	 }
</code></p>

**ExceptionControllerAdvice.java**
<p><code>
	@RestControllerAdvice
	public class ExceptionalCotrollerAdvice{
		private static final Log----------;
		@Autowired
		private Environment environment;

  		@Exceptionhandler(PatientAdmissionException.class)
    		public ResponseEntity<*ErrorInfo> patientAdmissionExceptionHandler(){
      				-----------
	  	}

      		@ExceptionHandler(Exception.class)
		public responseEntity<*ErrorInfo> generalExceptionHandler(){
  			--------------
     		}

       		@ExceptionHandler({MethodArgumentNoValidException.class, ConstraintViolationException.class})
	 	public responseEntity<*ErrorInfo>  validatorExceptionHandler(){
   				--------------
       		}
</code></p>

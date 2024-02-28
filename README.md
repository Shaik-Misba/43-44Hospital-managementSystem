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

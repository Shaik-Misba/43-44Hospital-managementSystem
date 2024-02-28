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

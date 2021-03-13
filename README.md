# TM4J Junit Integration

This is an example project on how to use the integration of TM4J - Test Management for Jira and automated tests when using JUnit.

In order to achieve that, you need to annotate the JUnit methods with `@TestCase`.

## Usage

Firstly, you need to add the dependency to your pom file, to get access to the JUnit listener and the annotations:

```
<dependencies>
    <dependency>
        <groupId>com.adaptavist</groupId>
        <artifactId>tm4j-junit-integration</artifactId>
        <version>1.0.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

Also, you'll need to register the TM4J JUnit Listener. This listener is responsible for generating the correct JSON
output file needed for uploading to Test Management for Jira.

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.0</version>
            <configuration>
                <properties>
                    <property>
                        <name>listener</name>
                        <value>com.adaptavist.tm4j.junit.ExecutionListener</value>
                    </property>
                </properties>
            </configuration>
        </plugin>
    </plugins>
</build>
```

The next step is to annotate your JUnit tests with @TestCase.

The @TestCase(key = "JQA-T1") annotation maps the test method to an existing test case in Test Management for Jira, by matching its key.

The @TestCase(name = "Sum Two Numbers") annotation adds a pretty name to the test case. This will map this test method to an existing test case in Test Management for Jira, by matching its name. If the test case doesn't exist in Test Management for Jira, then a new one can be automatically created using this name, when the results are uploaded.

```
public class CalculatorSumTest {

    @Test
    @TestCase(key = "JQA-T1")
    public void sumTwoNumbersAndPass() {
        Calculator calculator = new Calculator();
        assertEquals(5, calculator.sum(3, 2));
    }

    @Test
    @TestCase(key = "JQA-T2")
    public void sumTwoNumbersAndFail() {
        Calculator calculator = new Calculator();
        assertNotEquals(2, calculator.sum(1, 2));
    }

    @Test
    public void notMappedToTestCaseAndPass() {
        Calculator calculator = new Calculator();
        assertEquals(3, calculator.sum(1, 2));
    }

    @Test
    @TestCase(name = "Mapped to a Test Case Name and Pass")
    public void mappedToATestCaseNameAndPass() {
        Calculator calculator = new Calculator();
        assertEquals(4, calculator.sum(2, 2));
    }

}

```

Now, you can run your tests with `mvn test` and the TM4J test execution result file will be generated in the same execution folder.

## Support

For any issues or enquires please get in touch with the TM4J - Test Management for Jira team at SmartBear using the [support portal](https://productsupport.adaptavist.com/servicedesk/customer/portal/27).

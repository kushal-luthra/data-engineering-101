- PyTest is a python unit testing framework. <br>
- It provides the ability to create Tests, Test Modules, Test Classes, and Test Fixtures. <br>
- It uses the built-in python assert statement which makes implementing unit tests much simpler than other frameworks. <br>
- It also adds many useful command line arguments to help specify what tests should be run and in what order. <br>

#### Creating a Test
- Tests are python functions with “test” at the beginning of the function name. <br>
- Tests do verification of values using the standard python assert statement. <br>
- Similar tests can be grouped together by including them in the same module or class <br>

```
# test_SomeFunction.py
def test_SomeFunction():
 assert 1 == 1

```

#### Test Discovery
- Pytest will automatically find your tests when you run it from the command line using several naming rules for the test files, test classes, and test functions. <br>
- Test function names should begin with “test”. <br>
- Classes with tests in them should have the word “Test” with a capital T at the beginning of the class name. These classes should also have no “init” method. <br>
- The filenames for test modules should start with “test_” or end with “_test”. <br>

#### XUNIT Style setup and tear down
- One key feature of all unit test frameworks is providing the ability to execute setup code before and after the tests. Pytest provides this capability with both XUnit
style setup/teardown functions and with Pytest fixtures.
- The XUnit style setup and teardown functions allow you to execute code before and after: Test modules <click>, Test Functions <click>, Test Classes <click>, and Test
Methods in Test Classes.
- Using these setup and teardown functions can help reduce code duplication by letting you specify setup and teardown code once at each of the different levels as
necessary rather than repeating the code in each individual unit test. This can help keep your code clean and manageable.
- Lets look at some examples to see how this works.

```    
def setup_module():
def teardown_module():
def setup_function():
def teardown_function():
def setup_class():
def teardown_class():
def setup_method():
def teardown_method():
```

``` 
To run below code use command : pytest -v -s 
class TestClass:

    @classmethod
    def setup_class(cls):
        print('\n setup up TestClass')

    @classmethod
    def teardown_class(cls):
        print('\n tear down TestClass')


    def setup_method(self, method):
        if method==self.test1:
            print ('\n setting up test1')
        elif method==self.test2:
            print ('\n setting up test2')
        else:
            print('\n setting up unkown test')

    def teardown_method(self, method):
        if method==self.test1:
            print ('\n tear down  test1')
        elif method==self.test2:
            print ('\n tear down test2')
        else:
            print('\n tear down unkown test')

    def test1(self):
        print ('\n Executing Test1')
        assert True

    def test2(self):
        print ('\n Executing Test2')
        assert True
```

#### PyTest Fixtures
- Like the XUnit style of setup and teardown functions, Test fixtures allow for re-use of code across tests by specifying functions that should be executed before the
unit test runs.
- Specifying that a function is a test fixture is done by applying the “pytest.fixture” decorator to the function.
- Individual unit tests can specify they want to use that function **_by specifying_** it in their parameter list or **_by using_** the “pytest.mark.usefixture” decorator.
- The fixture can also set its **_autouse parameter_** to true which will cause all tests in the fixture’s scope to automatically execute the fixture before the test executes.
- Lets look at an example.
``` 
@pytest.fixture():
def math():
    return Math()
def test_Add(math):
    assert math.add(1,1) == 2 
```

``` 
import pytest

@pytest.fixture()
def setup():
    print ('\n Setup')

# below setup fixture gets called before test1 executes
# This is because we passed setup as parameter to out test1() function
def test1(setup):
    print ('Executing test1')
    assert True

# below setup fixture doesn't gets called before test2 executes
def test2():
    print ('Executing test2')
    assert True

# below setup fixture gets called before test1 executes
# This is because we passed setup via decorator using below syntax.
@pytest.mark.usefixtures("setup")
def test3():
    print ('Executing test3')
    assert True

```
Output <br>
``` 
test_fixture_example1.py::test1 
 Setup
Executing test1
PASSED
test_fixture_example1.py::test2 Executing test2
PASSED
test_fixture_example1.py::test3 
 Setup
Executing test3
PASSED
```

```
Note - It would be cumbersome to pass fixture to each test manually.
Instead, we use autouse functionality so that fixture gets called before each test.
Eg - 
    @pytest.fixture(autouse=True)
    def setup():
        print ('\n Setup')
```


###### Test Fixture Teardown
- Often there is some type of teardown or cleanup that a test, class, or module need to perform after testing has been completed.
- Each test fixture can specify their own teardown code that should be executed.
- There are two methods of specifying a teardown code for a test fixture: 
  - The “**yield**” keyword and 
  - The request-context object’s “**addfinalizer**” method.
<br><br>

**Test Fixture Teardown - Yield**<br>
- The yield keyword is the simpler of the two options for teardown code.
- The code after the yield is executed after the fixture goes out of scope.
- The yield keyword is a replacement for return and any return values should be passed to it.
``` 
@pytest.fixture():
def setup():
     print(“Setup!”)
     yield
     print(“Teardown!”) 
```
<br><br>
**Test Fixture Teardown - addfinalizer** <br>
- The addfinalizer method of adding teardown code is a little more complicated but also a little more capable than the yield statement.
- With the addfinalizer method one or more finalizer functions are added via the request-context’s addfinalizer method.
- One of the big differences between this method and the yield keyword method is that this method allows for multiple finalization functions to be specified.
- Now lets take a look at some examples.
``` 
@pytest.fixture():
def setup(request):
     print(“Setup!”)
     def teardown:
     print(“Teardown!”)
     request.addfinalizer(teardown) 
```

Example of 2 teardown methods <br>
``` 
import pytest

@pytest.fixture()
def setup1():
    print ('\n Setup 1')
    yield
    print ('\n Teardown 1')


@pytest.fixture()
def setup2(request):
    print ('\n Setup 2')

    def teardown_2_a():
        print ('\n Teardown 2A called')

    def teardown_2_b():
        print ('\n Teardown 2B called')

    request.addfinalizer(teardown_2_a)
    request.addfinalizer(teardown_2_b)

@pytest.mark.usefixtures("setup1")
def test1():
    print ('Executing test1')
    assert True

@pytest.mark.usefixtures("setup2")
def test2():
    print ('Executing test2')
    assert True

```
Output <br>
``` 
test_fixture_example2.py::test1 
 Setup 1
Executing test1
PASSED
 Teardown 1

test_fixture_example2.py::test2 
 Setup 2
Executing test2
PASSED
 Teardown 2B called

 Teardown 2A called
```



##### Test Fixtures Scope
- Which tests a test fixture applies to and how often it is run depends on the fixture’s scope.
- Test fixtures have four different scopes:
  - Function - Run the fixture once for each test
  - Class - Run the fixture once for each class of tests
  - Module - Run once when the module goes in scope
  - Session - The fixture is run when pytest starts.
- By default, the scope is set to function and this specifies that the fixture should be called for all tests in the module.
- Class scope specifies the test fixture should be executed once per test class.
- Module scope specifies that the fixture should be executed once per module.
- Session scope specifies that the fixture should be executed once when PyTest starts.

Eg - code 1 : test_fixture_example3_scope_of_fixtures.py <br>
``` 
import pytest

@pytest.fixture(scope='session', autouse=True)
def setupSession():
    print ("\n Setup Session")

    yield
    print("\n Teardown Session")


@pytest.fixture(scope='module', autouse=True)
def setupModule():
    print ("\n Setup Module")

    yield
    print("\n Teardown Module")


@pytest.fixture(scope='function', autouse=True)
def setupFunction():
    print ("\n Setup Function")

    yield
    print("\n Teardown Function")

def test1():
    print ('Executing Test 1')
    assert True

def test2():
    print ('Executing Test 2')
    assert True
```

Eg - code 2 : test_fixture_example4_scope_of_fixtures.py <br>
``` 
import pytest

@pytest.fixture(scope='module', autouse=True)
def setupModule():
    print ("\n Setup Module2")

    yield
    print("\n Teardown Module2")


@pytest.fixture(scope='class', autouse=True)
def setupClass():
    print ("\n Setup Class2")

    yield
    print("\n Teardown Class2")

@pytest.fixture(scope='function', autouse=True)
def setupFunction():
    print ("\n Setup Function2")

    yield
    print("\n Teardown Function2")

class TestClass:
    def test1(self):
        print ('Executing Test 1')
        assert True

    def test2(self):
        print ('Executing Test 2')
        assert True
```
When both are together run for pytest, the output is -> <br>
<br>
``` 
test_fixture_example3_scope_of_fixtures.py::test1 
 Setup Session

 Setup Module

 Setup Function
Executing Test 1
PASSED
 Teardown Function

test_fixture_example3_scope_of_fixtures.py::test2 
 Setup Function
Executing Test 2
PASSED
 Teardown Function

 Teardown Module

test_fixture_example4_scope_of_fixtures.py::TestClass::test1 
 Setup Module2

 Setup Class2

 Setup Function2
Executing Test 1
PASSED
 Teardown Function2

test_fixture_example4_scope_of_fixtures.py::TestClass::test2 
 Setup Function2
Executing Test 2
PASSED
 Teardown Function2

 Teardown Class2

 Teardown Module2

 Teardown Session
```


##### Test Fixture Return Objects and Params
- PyTest Test Fixtures allow you to optionally return data from the fixture that can be used in the test.
- The optional params array argument in the fixture decorator can be used to specify one or more values that should be passed to the test.
- When a params argument has multiple values then the test will be **called once with each value**.
- Let's look at a working example. 
``` 
import pytest

@pytest.fixture(params=[1,2,3])
def setup(request):
    return_val = request.param
    print ('\n Setup return_val = {}'.format(return_val))

    return return_val

# Note: we didn't use  @pytest.mark.usefixtures("setup") here as we needed to access value of setup fixture.
def test1(setup):
    print ('Executing test1')
    print ('Setup return value inside test function = ',format(setup))
    assert True
```
Output 
``` 
test_fixture_example5_return_objects_and_params.py::test1[1] 
 Setup return_val = 1
Executing test1
Setup return value inside test function =  1
PASSED
test_fixture_example5_return_objects_and_params.py::test1[2] 
 Setup return_val = 2
Executing test1
Setup return value inside test function =  2
PASSED
test_fixture_example5_return_objects_and_params.py::test1[3] 
 Setup return_val = 3
Executing test1
Setup return value inside test function =  3
PASSED
```


#### Using Assert and Testing Exceptions
##### Using the assert Statement
- Pytest allows the use of the built in python assert statement for performing verifications in a unit test.
- The normal comparison operators can be used on all python data types: <, >,<=, >=, ==, and !=
- Pytest expands on the messages that are reported for assert failures to provide more context in the test results.
``` 
import pytest
def test_IntAssert():
  assert 1 == 1

def test_StrAssert():
  assert "str" == "str"

def test_floatAssert():
  assert 1.0 == 1.0

def test_arrayAssert():
  assert [1,2,3] == [1,2,3]

def test_dictAssert():
  assert {"1":1} == {"1":1}
```
Output 
``` 
collected 6 items                                                                                                                                                                                                      

src/test/test_assert_example.py::test_IntAssert PASSED
src/test/test_assert_example.py::test_StrAssert PASSED
src/test/test_assert_example.py::test_floatAssert PASSED
src/test/test_assert_example.py::test_arrayAssert PASSED
src/test/test_assert_example.py::test_dictAssert PASSED
src/test/test_exception_example.py::test_exception PASSED
```

**Comparing Floating Point Values** <br>
- Validating floating point values can sometimes be difficult as internally the value is stored as a series of binary fractions.
- Because of this some comparisons that we’d expect to pass will fail.
- Pytest provides the “approx” function which will validate that two floating point values are “approximately” the same value as each other to within a default tolerance of 1 time E to the -6 value.
``` 
from pytest import approx


# Failing Test
def test_BadFloatCompare():
   assert (0.1 + 0.2) == 0.3


# Passing Test
def test_GoodFloatCompare():
   val = 0.1 + 0.2
   assert val == approx(0.3) 
```

##### Verifying Exceptions
- In some test cases we need to verify that a function raises an exception under certain conditions.
- Pytest provides the raises helper to perform this verification using the “with” keyword.
- When the “raises” helper is used the unit test will fail if the specified exception is not thrown in the code block after the “raises line.
``` 
from pytest import raises

def raisesValueException():
    raise ValueError

def test_exception():
    with raises(ValueError):
        raisesValueException()
```

#### PyTest Command Line Arguments
##### Specifying What Tests Should Run
- By default, Pytest runs all tests that it finds in the current working directory and sub-directory using the naming conventions for automatic test discovery.
- There are several pytest command line arguments that can be specified to try and be more selective about which tests will be executed.
  - moduleName - You can simply pass in the module name to execute only the unit tests in one particular module.
  - DirectoryName - You can also simply pass in a directory path to have pytest run only the tests in that directory.
  - You can use the “-k” option to specify an evaluation string based on keywords such as: module name, class name, and function name.
  - You can use the “-m” option to specify that any tests that have a “pytest.mark” decorator that matches the specified expression string will be executed.

Examples <br>
```
# 1 : DirectoryName
pytest -v -s path_to_my_directory/

#2 : -k option : runs all test having keyword test1 inside them.
pytest -v -s -k "test1"

#3 : -k option : runs all test having keyword test1 or test2 inside them.
pytest -v -s -k "test1 or test2"


#4 : -m option
define your tests by adding pytest.mark as shown below->

@pytest.mark.test1
def test1():
    print ('executing test1')
    assert True

@pytest.mark.test2
def test2():
    print ('executing test2')
    assert True

pytest -v -s -m "test1 or test2"         
```

Some additional command line arguments that can be very useful. <br>
- -v option specifies that verbose output from pytest should be enabled.
- -q option specifies the opposite. It specifies that the tests should be run quietly (or minimal output). <br>This can be helpful from a performance perspective when
you’re running 100’s or 1000’s of tests.
- -s option specifies that PyTest should NOT capture the console output.
- —ignore option allows you to specify a path that should be ignore during test discovery.
- —maxfail option specifies that PyTest should stop after N number of test failures.
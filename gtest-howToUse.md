   
	1.用法流程
	2.经常用到的Macros 
	3.比较重要的注意点
	4.不常用的功能&微末注意点

----------

# Google Test #

TestProgram -> Test Suite -> TestCase

TestProgram -> Test Case -> Test

1. assertion's result :
Can be **success, nonfatal failure, or fatal failure**. If a **fatal failure** occurs, it **aborts the current function**
	> * EXPECT_ --- Nonfatal
	> * ASSERT_ ---Fatal


2. Test Case/Test Fixture :
 	> * reflect the structure of the tested code
 	> * share common objects and subroutines

----------

## 1.Macros---Usually used ##
EXPECT_  or  ASSERT_

	TRUE(condition)
	EQ(expected,ac tual)
	NE(val1,val2)
	LT(val1,val2)
	LE(..)
	GE(..)
	GT(..)
	STREQ(expected,actual)  EXPECT_STREQ(NULL,c_str)
	STRNE(val1,val2)
	FLOAT_EQ(a,b)
	DOUBLE_EQ(a,b)
	NEAR(a,b,error)
	
	SUCCEEDED()
	FAIL()
	ADD_FAILURE()
	ADD_FAILURE_AT(filepath,lineNumber)
	
	ASSERT_PRED1(pred1, val1);判断TRUE FALSE 并在失败时候打印每个参数的值
	
	AssertionResult AssertionSuccess  AssertionFailure
	
  The one constraint is that **assertions that generate a fatal failure (FAIL* and ASSERT_*) can only be used in void-returning functions**.
  
  Note: Constructors and destructors are **not** considered void-returning functions, according to the C++ language specification, and so you may not use fatal assertions in them. You'll get a compilation error if you try. A simple workaround is to _transfer the entire body of the constructor or destructor to a private void-returning method_. However, you should be aware that a fatal assertion failure in a constructor does **not** terminate the current test, as your intuition might suggest; it merely returns from the constructor early, possibly leaving your object in a partially-constructed state. Likewise, a fatal assertion failure in a destructor may leave your object in a partially-destructed state. Use assertions carefully in these situations!
	
	TEST()
	TEST_F()
	TEST_P()
	
	SCOPED_TRACE(message);
	
> ASSERT_NO_FATAL_FAILURE(statement);
> EXPECT_NO_FATAL_FAILURE(statement);
> statement doesn't generate any new fatal failures in the current thread.
> HasFatalFailure() in the ::testing::Test class returns true if an assertion in the current test has suffered a fatal failure. This allows functions to catch fatal failures in a sub-routine and return early.
> HasNonfatalFailure()   ;
> HasFailure();
	
## TestCase:
1. Derive a class from **::testing::Test** . Start its body with **protected or public**: as we'll want to access fixture members from sub-classes.
2. SetUp(); TearDown()

	> PS: 最好是构造函数或者析构函数去初始化，因为子类调用父类的构造函数和析构函数。
		
## Test

1. Create a fresh test fixture at runtime
2. Immediately initialize it via SetUp() ,
3. Run the test
4. Clean up by calling TearDown()
5. Delete the test fixture. **Note** that different tests in the same test case have different test fixture objects, and Google Test always deletes a test fixture before it creates the next one. Google Test does not reuse the same test fixture for multiple tests. Any changes one test makes to the fixture do not affect other tests.	
When invoked, the RUN_ALL_TESTS() macro:

1. Saves the state of all Google Test flags.
2. Creates a test fixture object for the first test.
3. Initializes it via SetUp().
4. Runs the test on the fixture object.
5. Cleans up the fixture via TearDown().
6. Deletes the fixture.
7. Restores the state of all Google Test flags.
8. Repeats the above steps for the next test, until all tests have run.
In addition, if the text fixture's constructor generates a fatal failure in step 2, there is no point for step 3 - 5 and they are thus skipped. Similarly, if step 3 generates a fatal failure, step 4 will be skipped.

Important: You must not ignore the return value of RUN_ALL_TESTS(), or gcc will give you a compiler error. The rationale for this design is that the automated testing service determines whether a test has passed based on its exit code, not on its stdout/stderr output; thus your main() function must return the value of RUN_ALL_TESTS().

Also, you should call RUN_ALL_TESTS() only once. Calling it more than once conflicts with some advanced Google Test features (e.g. thread-safe death tests) and thus is not supported.

自定义作为宏的参数的类型的输出：
void PrintTo(const Bar& bar, ::std::ostream* os);
## 2.CascadeSDK_UnitTest Project ##

## 3.Gtest Flags ##
**command line:**

	--gtest_XXX

**program:**

	::testing::GTEST_FLAG(XXX)
	
**flags:**
	
> * --gtest_list_tests
> * --gtest_filter=*TestcaseName.TestName*   	:   -
> A test matches the filter if and only if it matches any of the positive patterns but does not match any of the negative patterns.
	
> * disable test:   DISABLED_*testName*   
> * --gtest_repeat = *times*
> * --gtest_break_on_failure
> * --gtest_ shuffle=1  --gtest_ random_ seed=SEED
> * --gtest_ output=*"xml:filepath"*
> * --gtest_catch_exceptions=0
	

## 4.Macros And Functions---Not Imp ##

----------

# Google Mock #

----------

## How to Use ##

----------

# Brain Holes #

----------
## 注意点
Tests should be independent and repeatable. 




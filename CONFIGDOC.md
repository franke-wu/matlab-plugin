## Contents

-  [Configure Plugin for Freestyle Project](#configure-plugin-for-freestyle-project)
	- [Freestyle Project with Automatic Test Mode](#option-1-freestyle-project-with-automatic-test-mode)
	- [Freestyle Project with Custom Test Mode](#option-2-freestyle-project-with-custom-test-mode)
-  [Configure Plugin for Multi-Configuration Project](#configure-plugin-for-multi-configuration-project)
	- [Multi-Configuration Project with Automatic Test Mode](#option-1-multi-configuration-project-with-automatic-test-mode)
	- [Multi-Configuration Project with Custom Test Mode](#option-2-multi-configuration-project-with-custom-test-mode)
	
## Configure Plugin for Freestyle Project
To configure the plugin for a freestyle project, select **Run MATLAB Tests** from the **Add build step** list. Then, enter the value returned by the **matlabroot** function in the **MATLAB root** field.

  ![new_add_build_step](https://user-images.githubusercontent.com/47204011/55624172-be54a100-57c2-11e9-9596-52d3a60ee467.png)
  
  ![new_enter_matlabroot](https://user-images.githubusercontent.com/51316279/70772996-28830400-1d9c-11ea-92d1-db9f054c304c.png)
  
### Option 1: Freestyle Project with Automatic Test Mode
With the Jenkins plugin for MATLAB, you have the option to run your tests in either *automatic* or *custom* mode. The automatic test mode employs a default setting to run tests written using the MATLAB Unit Testing Framework and/or Simulink Test. If your source code is organized as files and folders within a project, the plugin will consider any test files in the project that have been tagged as **Test**. If your code does not leverage a project or uses a MATLAB version prior to R2019a, the plugin will consider all tests in the current Jenkins workspace including the subfolders. 

If you use a source code management (SCM) system such as Git, then your job must include the appropriate SCM configuration to check out the code before it can invoke the MATLAB plugin. If you do not use any SCM systems to manage your code, then an additional build step is required to ensure that the code is available in the Jenkins workspace before the build starts.

The automatic test execution feature of the plugin enables you to generate different types of test artifacts. To publish the test results, you can use these artifacts with other Jenkins plugins. To configure the Jenkins build where MATLAB tests run automatically, follow these steps.

1) From the **Test mode** drop-down list, select the **Automatic** option (**Automatic** is the default testing mode).
  
  ![new_select_automatic_option](https://user-images.githubusercontent.com/51316279/70773784-6719be00-1d9e-11ea-9a6c-7b277d63e10a.png)
  
2) Select your desired test artifacts.

  ![new_select_all_test_artifacts](https://user-images.githubusercontent.com/51316279/70773293-0dfd5a80-1d9d-11ea-8d24-28407e67c8a5.png)

  The selected artifacts will be saved in the **matlabTestArtifacts** folder of the Jenkins workspace.

  ![Workspace01](https://user-images.githubusercontent.com/47204011/55470859-1e621080-5626-11e9-98f2-044144272643.JPG)
  
  ![Test_artifacts](https://user-images.githubusercontent.com/51316279/70776181-5bc89180-1da2-11ea-92e6-4c71deab7199.png)
  
  If you do not select any of the test artifact check boxes, the **matlabTestArtifacts** folder will not be created in the workspace. However, tests will still run and potential test failures will fail the build. 

  The **Automatic** test mode results in a MATLAB script file named **runMatlabTests.m** in the Jenkins workspace. The plugin uses this file to run the tests and generate the test artifacts. You can review the contents of the script to understand the testing workflow.

  ![Workspace01](https://user-images.githubusercontent.com/47204011/55470859-1e621080-5626-11e9-98f2-044144272643.JPG)

### Option 2: Freestyle Project with Custom Test Mode
This option enables you to develop your custom MATLAB commands for running tests. To configure the Jenkins build where you can customize the MATLAB test execution, follow these steps.

1) From the **Test mode** drop-down list, select the **Custom** option.

  ![new_select_custom](https://user-images.githubusercontent.com/51316279/70775386-42731580-1da1-11ea-9523-41c83443ee5b.png)

2) Enter your commands in the **MATLAB command** field. If you specify more than one MATLAB command, use a comma or semicolon to separate the commands. The build will fail if the execution of any command results in an error.

  ![new_custom_runtest_command](https://user-images.githubusercontent.com/47204011/55624949-096fb380-57c5-11e9-8711-98baf91816c0.png)

  **Note:** If you need several MATLAB commands to run your tests, consider writing a MATLAB script or function as part of your repository and executing this script or function instead. Test artifacts are not autogenerated if you choose to run tests using custom MATLAB commands. You can generate your desired test artifacts by configuring the test runner in the script or function that you invoke from the **MATLAB command** field.

  ![new_custom_script_example](https://user-images.githubusercontent.com/47204011/55625021-32904400-57c5-11e9-86b7-478b930796c0.png)

## Configure Plugin for Multi-Configuration Project
In addition to freestyle projects, the Jenkins plugin for MATLAB supports [multi-configuration (matrix) projects](https://wiki.jenkins.io/display/JENKINS/Building+a+matrix+project). Multi-configuration projects are useful when builds include similar steps, for example when the same test suite should run on different platforms (e.g., Windows, Linux, and Mac) or using several MATLAB versions.

![image](https://user-images.githubusercontent.com/47204011/62458632-0e586a00-b79b-11e9-8611-3671adb8c289.png)

As in a freestyle project, you can run your tests in automatic or custom mode within a multi-configuration project. The configuration requires you to specify the location where MATLAB is installed as well as the test execution mode. You should also add user-defined axes in the **Configuration Matrix** to specify the duplicating build steps. 

### Option 1: Multi-Configuration Project with Automatic Test Mode

To configure the plugin for a matrix build where tests run automatically in multiple MATLAB versions, create a multi-configuration project and follow these steps.

1) Add a user-defined axis in the **Configuration Matrix** to represent the MATLAB versions in the build. Specify the name of the axis in the **Name** field and its values in the **Values** field. Separate the elements in the **Values** field with a space. In this example, four MATLAB versions are specified, which will be used to run the same set of tests.

![image](https://user-images.githubusercontent.com/47204011/62603081-c2c8cc00-b912-11e9-83a4-c5462f58f607.png)

2) In the **Run MATLAB Tests** section of the project, include the user-defined axis name in the **MATLAB root** field to specify the locations where MATLAB is installed. In this example, **$VERSION** will be replaced by one axis value per build step.

![image](https://user-images.githubusercontent.com/51316279/70773424-72b8b500-1d9d-11ea-97ca-807ecd6c4e47.png)

You can select the test artifact check boxes when tests run automatically. Once you have made your selections, save your settings and run the build.

### Option 2: Multi-Configuration Project with Custom Test Mode

To configure the matrix build where you can customize the MATLAB test execution, create a multi-configuration project and follow these steps.

1) Add a user-defined axis in the **Configuration Matrix** to represent the MATLAB versions in the build. 

![image](https://user-images.githubusercontent.com/47204011/62603081-c2c8cc00-b912-11e9-83a4-c5462f58f607.png)

2) Add another user-defined axis using the **Add axis** button. In this example, the **TEST_TAG** axis specifies the possible test tags for a group of test elements.

![image](https://user-images.githubusercontent.com/47204011/62517774-b6c30880-b845-11e9-86a0-8344a281fb27.png)

3) In the **Run MATLAB Tests** section of the project, use the **VERSION** axis to specify the locations where MATLAB is installed.

![image](https://user-images.githubusercontent.com/51316279/70773424-72b8b500-1d9d-11ea-97ca-807ecd6c4e47.png)

4) From the **Test mode** drop-down list, select the **Custom** option. Use the second user-defined axis to create your commands and enter them in the **MATLAB command** field. Then, save your settings and run the build. 

![image](https://user-images.githubusercontent.com/47204011/62686681-cd529680-b9e2-11e9-82c1-c211f1740be4.png)

**Notes:**
1) For a user-defined axis named **VAR**,  **$VAR** and **${VAR}** are both valid formats for accessing the values.

2) A multi-configuration project creates a separate workspace for each user-defined axis value. If you specify the full paths to where MATLAB is installed as axis values, Jenkins fails to create separate workspaces and fails the build.	
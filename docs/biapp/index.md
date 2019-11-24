# Connect Applications

This lab contains the following tasks:

## 3. Connect BI Applications

Amazon Aurora is MySQL and PostgreSQL compatible relational database engine. This means any existing applications that work with MySQL and PostgreSQL will have drop-in compatibility with Amazon Aurora. For this workshop, we will be deploying a business intelligence (BI) application that is running on Amazon EC2 instance in each region, and connect them to the respective local endpoint of the Aurora Global Database, to achieve lower query latency.

For the purpose of this workshop, we will be using [Apache Superset (incubating)](https://superset.incubator.apache.org/) as the BI web application. Superset is an open source, business intelligence and data exploration platform designed to be visual, intuitive and interactive.

<details>
<summary>Connecting to EC2 Instances - Additional Details</summary>
Those who have been familiar with AWS for a while may remember that connecting to a remote Amazon EC2 instance requires opening inbound SSH or Powershell ports, provisioning SSH keys and management of certificates. With AWS Systems Manager Session Manager, you can connect to an EC2 instance with just few clicks and experience a secure browser-based CLI, without having to provision or create SSH keys.
</details>

#

>  **`Region 1 (Primary)`** 


1. In the AWS Management Console, ensure that you are working within your assigned primary region. Use the Service menu and click on **Systems Manager** under Management and Governance or simply type **Systems Manager** into the search bar. This will bring up the AWS Systems Manager console.

1. Within the Systems Manager console, select **Session Manager** on the left menu. Click on the **Start Session** button.

1. You should now see your EC2 hosts that are running which you can connect to. Select ``!region1-superset-host``, then click on the **Start Session** button. This will open a new browser tab with the terminal session. Copy and paste the following commands into the terminal, and press Enter after pasting.

   1. Let's start with enabling bash on the terminal view

      ```
      source ~/.bashrc
      ```
   1. We will now create an admin user for the Apache Superset application

      ```
      fabmanager create-admin --app superset
      ```

      You will be prompted for the following:
       * Username (press enter for default)
       * First Name (press enter for default)
       * Last Name (press enter for default)
       * Email (press enter for default)
       * Password (enter your password, don't forget this!)
       * Repeat for Confirmation (confirm your password)

    1. Once complete you will receive the message that admin has been created

       ![Superset Commands](./superset-flask.png)

    1. Next, we will run the following commands to initiate and run the Superset application in the background. Include the final ampersand "&" while copying and pasting.

       ```
       superset db upgrade
       superset load_examples
       superset init
       nohup gunicorn -b 0.0.0.0:8088 --limit-request-line 0 --limit-request-field_size 0 superset:app &
       ```

    1. The application will take a minute or two to build samples and initialize. Once you see the message similar to those below, Superset is running, with the service running by a web server on TCP port 8088.

       ```
       [2019-xx-xx 00:00:00 +0000] [11827] [INFO] Listening at: http://0.0.0.0:8088 (11827)
       [2019-xx-xx 00:00:00 +0000] [11827] [INFO] Using worker: sync
       [2019-xx-xx 00:00:00 +0000] [11831] [INFO] Booting worker with pid: 11831
       ```

1. Return to your AWS Management Console. Use the Service menu and click on **CloudFormation** or simply type **CloudFormation** into the search bar.

1. Click on **Stacks**, and select the stack that you have deployed for this particular region. Click on the **Outputs** tab.

1. Locate the value for the key **supersetURL**, and copy to your clipboard. This value should be similar to 

    ```ec2-12-34-56-78.<xx-region-x>.compute.amazonaws.com:8088/```

1. Open a new browser tab or window. Paste the URL value into your address bar, then press enter.

1. You should see the login page for Superset. Type in ```admin``` for **Username** and the password you have entered from previous setup step.

    ![Superset Login](./superset-login.png)

1. If login is successful, you will then be taken to the Superset main dashboard. Congratulations!

# 


>  **`Region 2 (Secondary)`** 

1. Repeat steps above for Secondary region.
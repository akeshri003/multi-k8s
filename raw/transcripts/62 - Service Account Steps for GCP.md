Updated Service Account steps for new GCP UI

_updated 3-11-2021_

Part of the Google Cloud interface has seen some changes in the past few weeks, so, here is an updated walkthrough with screenshots to generate a Service Account.

1. Click the Hamburger menu on the top left-hand side of the dashboard, find **IAM & Admin**, and select **Service Accounts**. Then click the **CREATE SERVICE ACCOUNT** button.  
      
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-12_01-40-21-077c7465a3699388d9820554700c645e.png)
    
2. In the form that is displayed, set the **Service account name** to **travis-deployer** (step 1), then click the **CREATE** button (step 2).  
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-12_01-42-30-30150dbec735d6d44f9bba46b615bd93.png)
    
3. Click in the **Select a role** filter and scroll down to select **Kubernetes Engine** and then **Kubernetes Engine Admin**.  
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-12_01-45-18-afd0538f12465f3cfc9eeee5b1c70f41.png)
    
      
    
4. Make sure the filter now shows **Kubernetes Engine Admin** and then click **CONTINUE  
    **
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-12_01-47-08-e489e1a36ee49ffbe15636874e527617.png)
    
5. The Grant users access form is optional and should be skipped. Click the **DONE** button.  
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-12_01-48-55-541bc0b630d5d79cdae2ac939943142a.png)
    
6. You should now see a table listing all of the service accounts including the one that was just created. Click the **three dots** to the right of the service account you just created. Then select **Manage Keys** in the dropdown.
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-12_01-53-13-f75858dcf284b42a6b3df05f8bc35423.png)
    
7. In the **Keys** dashboard, click **ADD KEY** and then select **Create new key**.  
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-12_01-55-46-da5136afb4116caf968632b3c57f22d0.png)
    
8. In the **Create private key** dialog box, make sure **Key type** is set to **JSON,** and then click the **CREATE** button.  
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-12_01-58-10-d9f3af9d915297f9302ec1de07ebdfee.png)
    
9. The JSON key file should now download to your computer.
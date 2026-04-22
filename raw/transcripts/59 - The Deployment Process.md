![[Screenshot 2026-04-20 at 3.13.33 PM.png]]

## GKE Creation Steps Using Autopilot

This lecture will contain all of the steps required to create a Kubernetes cluster with GKE. It will be regularly updated as the GCP UI changes.

  

1. Click the Hamburger menu on the top left-hand side of the dashboard.  
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2025-09-11_19-39-07-2b0d29576a99f85662d6672396e91e3a.png)
    
2. Click **Kubernetes Engine,** then **Clusters  
    **
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2025-09-11_19-39-07-c7a3c7a81b34e15903e74720616208e3.png)
    
      
    
3. Click the **ENABLE** button to enable the Kubernetes API for this project.  
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2025-09-11_19-39-07-42bee9627279a030ba28d89a435e68ca.png)
    
4. After a few minutes of waiting, clicking the **bell** icon in the top right part of the menu should show a **green** checkmark for **Enable services: container.googleapis.com  
    **
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2025-09-11_19-39-07-f1f8010aba20fc911274d0f013191593.png)
    
      
    
5. If you refresh the page, it should show a screen to create your first cluster. If not, click the hamburger menu and select **Kubernetes Engine** and then **Clusters**.  
    Once you see the screen below, click the **CREATE** button.  
      
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-03-11_23-28-16-3857eecbe547d323f0c5986e07944f31.png)
    
      
    
6. A **Create Cluster** dialog will open and default to Autopilot. This will be the cheaper option at around $5 per day and will require no setup or configuration. The Standard cluster creation will cost upwards of $350 per month. Give the cluster a name and click the **Create** button.
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2025-09-11_19-39-08-03004c6e0777307c28dec4bb3b4a0021.png)
    
      
    
7. After a few minutes, the cluster dashboard should load and your multi-cluster should have a **green** checkmark in the table.  
    
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2025-09-11_22-00-41-4464587523df50e4280843222ead30a5.png)
    

One detail that you will need to remember is that Autopilot uses **regions** and not **zones**. We will post a reminder edit to the lectures that make reference to the old standard cluster zones.

Also, Autopilot will automatically add or remove Nodes based on workload demand. So, you will not see any reference to Node Pools or Node configurations as you would have with a Standard cluster.

  

**Remember, as long as this cluster is running you will be billed real-life money! The autopilot cluster costs about $150 a month, the standard cluster can cost in excess of $350!**

**If you need to stop the course for _any reason_ remember to shut down this cluster.**  Directions for cleaning up the Google Cloud cluster can be found in a lecture at the very end of this course. Here's a direct link to it: [**https://www.udemy.com/docker-and-kubernetes-the-complete-guide/learn/v4/t/lecture/11684242?start=0**](https://www.udemy.com/docker-and-kubernetes-the-complete-guide/learn/v4/t/lecture/11684242?start=0)



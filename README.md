# Cloud-Serverless-Fast-Start-Minimizing-Cold-Start-Time
<h2>About</h2>
<p>This repository provides a solution for minimizing cold start time in Knative services using Kubernetes cronjobs and shell scripting. By implementing this solution, you can mitigate cold start times and achieve better performance for your serverless applications on Knative.</p>
<h2>Contents</h2>
<h2> Method 1: Container Startup Time Reduction using cronjob</h2>
<ul>
  <li><code>hello.yaml</code>: This Knative Service file is used to deploy functions.</li>
  <li><code>cronjob.yaml</code>: This Kubernetes cronjob calls the Knative function every 3 minutes to keep the pods warmed up.</li>
  <li><code>script.sh</code>: This shell script stops the cronjob when there are 3 successful jobs, ensuring optimal resource utilization.</li>
</ul>
<p>Use the provided files to implement the solution and achieve optimal cold start time for your Knative services.</p>

<h2>Steps to Run the Solution</h2>
<ol>
  <li>Install Knative and Kubernetes on your system. You can refer to the official documentation for installation instructions.</li>
  <li>Clone this repository to your local system. <br><code>git clone &lt;repository_url&gt;</code></li>
  <li>Navigate to the cloned repository directory. <br><code>cd &lt;repository_directory&gt;</code></li>
  <li>Deploy the Knative Service using the <code>hello.yaml</code> file. <br><code>kubectl apply -f hello.yaml</code></li>
  <li>Deploy the Kubernetes cronjob using the <code>cronjob.yaml</code> file. <br><code>kubectl apply -f cronjob.yaml</code></li>
  <li>Verify that the cronjob is running. <br><code>kubectl get cronjob</code></li>
  <li>Wait for a few minutes to allow the pods to warm up.</li>
  <li>Check the logs of the Knative service to confirm that it is running. <br><code>kubectl get svc</code></li>
  <li>Stop the cronjob using the <code>script.sh</code> file after 3 successful jobs. <br><code>./script.sh</code></li>
</ol>
<p>By following these steps, you can run the solution and mitigate cold start time for your Knative services.</p>

<h2>Method 2: Optimizing Cold Start Time for Knative Services with Image Caching</h2>
<p>Another approach to minimize cold start time in Knative Services is to use image caching. By caching the images used by your services, you can significantly reduce the time required to create new pods, resulting in faster cold start times.</p>
<p>Here's how you can implement image caching for your Knative Services:</p>
<ol>
  <li>Push the image to your container registry.</li>
  <li>Configure your Knative Service to use the image with the unique tag.</li>
  <li>Enable image caching in Knative by configuring the `imagePullPolicy` property of the container to `IfNotPresent` or `Always`. This tells Kubernetes to use the locally cached image if available, instead of downloading the image from the container registry.</li>
  <li>Set the `imagePullPolicy` property to `Always` for the first request to the service. This ensures that the latest version of the image is always used for the first request, even if it is already cached.</li>
  <li><code>cache.yaml</code>: This file defines the ImageCache resource and specifies the most commonly used image to cache.</li>
</ol>
<p>By implementing image caching for your Knative Services, you can achieve faster cold start times and better performance for your serverless applications.</p>

<h2>Steps to Implement Image Caching with <code>cache.yaml</code></h2>
<ol>
  <li>Install Knative and Kubernetes on your system. You can refer to the official documentation for installation instructions.</li>
  <li>Clone the repository containing the <code>cache.yaml</code> file to your local system. <br><code>git clone &lt;repository_url&gt;</code></li>
  <li>Navigate to the cloned repository directory. <br><code>cd &lt;repository_directory&gt;</code></li>
  <li>Apply the <code>cache.yaml</code> file to create the ImageCache resource. <br><code>kubectl apply -f cache.yaml</code></li>
  <li>Update your Knative pod to use the ImageCache resource. You can add the following annotation to your pod spec: <br>
    <code>annotations:
      caching.internal.knative.dev/image: &lt;cache_name&gt;</code><br>
    Replace &lt;cache_name&gt; with the name of the ImageCache resource created in step 4.
  </li>
  <li>Deploy the Knative pod to the Kubernetes cluster. <br><code>kubectl apply -f hello.yaml</code></li>
  <li>Compare the cold start and warm start time of your Knative Service by using the following command: <br>
    <code>time curl http://&lt;external-link&gt;</code><br>
    Replace &lt;external-link&gt; with the URL of your Knative Service. In a Knative service, a cold start happens when there are no pods running, and a warm start happens when the image is already cached and doesn't need to be pulled again. By using image caching, you can significantly reduce cold start time and accelerate pod creation.</li>
</ol>
<p>By following these steps, you can implement image caching with <code>cache.yaml</code> and accelerate pod creation for your Knative Services. You can also measure the impact of image caching on cold start time by comparing cold and warm start times with the <code>time curl</code> command.</p>

<!-- <h2>Method 3: Time Series Forecasting Approach to Minimize Cold Start Time in Cloud-Serverless</h2>
<p>For this approach, we used a SARIMA model to predict the traffic volume for a Knative service. The output of this forecasting model is then used to autoscale the pods beforehand, which helps to mitigate the impact of cold start in a graceful manner.</p>
<p>By using a time series forecasting approach, you can predict the future traffic for your Knative service and take proactive steps to prevent cold starts. This approach can help ensure that your service is always available and performs optimally for your users.</p> -->

<h2> Method 3: Using SARIMA Time Series Forecasting to Minimize Cold Start Time</h2>
<p>For this approach, we have developed a predictive model using SARIMA time series forecasting to predict traffic volume for a Knative service. We have implemented the model in the following files:</p>
<ul>
  <li><code>arima_model.pkl</code>: This file contains the trained SARIMA model, with its parameters stored in pickle format.</li>
  <li><code>ARIMA.ipynb</code>: This Jupyter notebook generates randomized data for the last 10 days and uses the SARIMA model to forecast future traffic.</li>
  <li><code>pba.py</code>: This Python script uses the trained <code>arima_model.pkl</code> file to forecast traffic based on the current datetime and predict and deploy autoscaling pods to prepare our pods for incoming traffic. This approach helps mitigate the impact of cold start in a graceful manner.</li>
</ul>
<p>By using a time series forecasting approach and implementing the SARIMA model, you can predict future traffic for your Knative service and take proactive steps to prepare your pods for incoming traffic. This approach can help ensure that your service is always available and performs optimally for your users.</p>
<p> Reference: A. P. Jegannathan, R. Saha and S. K. Addya, "A Time Series Forecasting Approach to Minimize Cold Start Time in Cloud-Serverless Platform," 2022 IEEE International Black Sea Conference on Communications and Networking (BlackSeaCom), Sofia, Bulgaria, 2022, pp. 325-330, doi: 10.1109/BlackSeaCom54372.2022.9858271. <p>

<h2>Steps to Implement SARIMA Time Series Forecasting for Minimizing Cold Start Time</h2>
<p>Here are the steps to run the <code>pba.py</code> script for implementing SARIMA time series forecasting to minimize cold start time:</p>
<ol>
  <li>Install Python and required libraries on your system. You can refer to the official documentation for installation instructions.</li>
  <li>Clone the repository containing the <code>pba.py</code> file to your local system.<br><code>git clone &lt;repository_url&gt;</code></li>
  <li>Navigate to the cloned repository directory.<br><code>cd &lt;repository_directory&gt;</code></li>
  <li>Run the <code>pba.py</code> script using the following command:<br><code>python pba.py</code></li>
  <li>The script will get the current timestamp and use the trained SARIMA model to predict the incoming traffic for the Knative service.</li>
  <li>Autoscaling pods will be deployed to prepare for the incoming traffic, mitigating the impact of cold start in a graceful manner.</li>
  <li>The script will sleep for 55 minutes before repeating the process.</li>
  <li>You can modify the time interval between runs by changing the sleep duration in the script.</li>
</ol>
<p>By following these steps, you can run the <code>pba.py</code> script and implement SARIMA time series forecasting to minimize cold start time for your Knative service. This approach can help ensure that your service is always available and performs optimally for your users.</p>

<h2>Results</h2>
<p>Using the methods described in this repository, we were able to significantly minimize the cold start time for our Knative services. Here are some of the results we achieved:</p>
<ul>
  <li>Method 1 - Container Startup Time Reduction: We were able to reduce the container startup time by up to 90% by optimizing the container image size and using the init container approach.</li>
  <li> Simple-api image of size 3 MB: </li>
<img width="634" alt="1" src="https://user-images.githubusercontent.com/57623274/228665997-a9db3d78-b7d2-457b-84d5-73a0229689ba.png">
<li> Java image of size 35 MB: </li>
<img width="1214" alt="2" src="https://user-images.githubusercontent.com/57623274/228666056-73388881-c53e-4efd-8154-56e7afcadcff.png">
<li>Method 2 - Image Caching: We were able to reduce the image layer pull time by up to 50% using image caching.</li>
<img width="1247" alt="Screenshot 2023-03-29 at 2 04 49 PM" src="https://user-images.githubusercontent.com/57623274/228667172-feb86435-5950-4173-87c5-4e1515168f0f.png">
</ul>

<h2>Team Members and Contributors</h2>
<ul>
  <li>Anish More</li>
  <li>Divaynsh Deshmukh</li>
</ul>
<p>We are grateful to all the contributors who have helped make this project possible. Thank you for your support and contributions!</p>
<p>If you are interested in contributing to this project, please feel free to reach out to us or submit a pull request. We welcome your contributions and look forward to collaborating with you!!</p>

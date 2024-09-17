🍔 Swiggy App Deployment with GitHub Actions 🍔
Welcome to the future of deployment! Today, we’ll be launching the Swiggy application using the power of GitHub Actions as our CI/CD tool — ditching Jenkins and CircleCI for something sleeker, faster, and more efficient! 💥

Ready to automate your infrastructure and deliver code like a boss? Let's dive in!

🎯 Road to Deployment — Step-by-Step Guide 🚀
Follow along, and you'll have Swiggy running in the cloud with the click of a button (or two) — everything managed with GitHub Actions. Check out the 9 steps below to get from zero to production like a pro! ⚡

Step 1 → Terraform Setup & AWS Configuration
First up, get your hands dirty by setting up Terraform. It’s time to turn your local machine into a coding powerhouse. You’ll configure AWS to allow Terraform to build and manage your cloud resources. 🌍

Step 2 → Build Your Infrastructure with Terraform
With Terraform in your toolkit, code out a simple, scalable infrastructure. You're writing the blueprint for your cloud resources and launching them at will — it’s like magic but real. 🎩✨

Step 3 → IAM Role for EC2
Security first! 🛡️ Create an IAM Role for your EC2 instances, so your GitHub Actions workflows can interact with AWS securely and seamlessly. It’s all about trust and permissions!

Step 4 → GitHub Actions + EC2 = 🔥
Time to connect the dots. Set up GitHub Actions to deploy your app to EC2 automatically. Every commit, every push will trigger your deployment pipeline and keep your app running smoothly. 🚀

Step 5 → SonarQube for Code Quality + DockerHub for Images
In this step, we introduce SonarQube to ensure every line of code you commit is clean and high-quality. Oh, and don’t forget DockerHub to store your Docker images for easy access and deployment! 🧼🛠️

Step 6 → EKS (Elastic Kubernetes Service) Cluster Setup
It’s Kubernetes time! Spin up an EKS cluster on AWS to run your Swiggy app in a scalable, containerized environment. Kubernetes will take care of the rest, keeping your app running smoothly no matter how many users swarm in. 🐝

Step 7 → Build and Push Docker Images Like a Pro
Now it’s time to containerize! Build your Docker images, push them to DockerHub, and automate the entire process with GitHub Actions. Each new feature is just a commit away from production. 🐳✨

Step 8 → Monitor with Prometheus & Grafana
What good is deployment if you can't monitor it? Set up Prometheus and Grafana to track your app’s performance in real time. Get beautiful dashboards and stay ahead of any issues before they become problems. 📊🔍

Step 9 → Tear It All Down (But Only If You Want To)
Done with the project or want to avoid AWS charges? Run your Terraform destroy command to clean up everything — like it never happened. 💨🧹

🎉 You Made It! 🎉
With these steps, you've mastered deploying the Swiggy app using GitHub Actions, Terraform, and AWS — all while maintaining quality with SonarQube and monitoring like a pro with Grafana. You’ve officially leveled up! ⚡🚀

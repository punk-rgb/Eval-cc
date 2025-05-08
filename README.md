# Eval-cc

git clone https://github.com/dockersamples/example-voting-app.git
cd example-voting-app

sudo kubectl apply -f k8s-specifications/
sudo kubectl get pods
sudo kubectl get services

Les pods doivent être en status running

## capture 

![Result app](result.png)

![Vote app](vote.png)
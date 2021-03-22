#lecture 
#cs287 

*page 4*
Basic structure of every model based RL

---
*page 5*
Advantages

---
*page 6*
Q:*Why not use all the time?*

Some instability and cannot reach the same level of performance of model-free methods. 

---
*page 7*

Overfitting real challenge:
- policy optimization overfitting over the quirks of the simulator and actually do bad on the real world. How do you measure that mismatch? You can train an ensemble, if the models disagree you should not have the policy go there. 

[[ME-TRPO]]

----
*page 15*
Lower performance:
- Imperfect dynamics learned
- the robustness of the policy due to the many ensables can lead to lower performance. We would like an adaptive policy that can adapt to any of the learned model/real world. It's gonna be a [[RNN]], the details are explained in [[MB-MPO]]

---
*page 24*
Problems so far:
- Not real time, compute clock pretty high.
- Short horizon, simulator can accumulate error
- Can we do it from pixels?

---
*page 25*
Traditional workflow 

---
*page 26*
A better workflow in an asynchronous fashion. 

---
*page 31*
Questions: 
1. Performance: we are not using all the data before we do data collection again
2. Regularization: the model is always changing 
3. Data exploration: if you always change the policy the way you collect the data is more exploratory 
4. Robustness to hyperparameters
5. Robustness to data collection frequency


---
*page 32*
1. Asynchronous version are much faster. Sample complexity is fairly comparable between synchronous and asynchronous. 

---
*page 36*
2. We can update the policy more frequently, the ensamble changes all the time and you avoid overfitting against a specific model. 

---
*page 38*
3. Improved exploratory data makes learning faster 

---
*page 42*
Does asynch work for real world robots? YES 

---
*page 51*

## Learning from pixels 
[[World Models]] proposes a natural approach. 
I don't want to go from current state to the next but rather let's try to compress the current state. The compression is done with a [[Variational Autoencoder]], the output the latent variable is used to learn the dynamics of the model. The lower dimensional variable helps to learn faster. 


When you want to learn something you should focus on self-supervised learing, that's where most of the data is.

---
*page 54*
VAE has 4 M params (self-supervised), RNN has 400K parameters (supervised learning), Policy has 1k params (RL). 

---
*page 55*
Check out [[Planet]]

---
*page 56*
I am gonna have a lineaer dynamics model in my latent space. 

---
*page 58*
[[Solar]] learn an embedding space where the dynamics is linear, and runs iLQR under the hood

---
*page 59*
Use a spacial-softmax layers, that outputs the coordinates of objects in your scene. 

---
*page 61*
[[Darla]], to output meaningful state you have to regularize the [[Variational Autoencoder]]. In this way you make your latent variable maximally indipendent. 

---
*page 62*
[[Causal InfoGAN]] accounts for the dynamics of the environments. It makes sure if you have two embedding states and you interpolate between them, when you recostruct the image sequence you should obtain a realistic sequence of states. 

---
*page 63*
[[Planet]] does multi step prediction. 

Often it works a lot better to run MPC against a model, a downsize is that it's a lot slower and imperfect model. 

---
*page 64*
Once you can do video prediction, you can run MPC, using cross entropy methods, to achieve the goal shown in the images. 

When a bottle brakes you don't need to know all the details, so it's better to predict in latent space rather than try to predict how the bottle brakes. 
With the latent variables, maybe we dont' need to reconstruct, we try to predict the action $a_t$ that will lead to the next state $s_{t + 1}$. Learn embedding that allow you to predict the actions.  

---
*page 70*
Learn to predict the feature that matter to him, fully [vision base](https://arxiv.org/pdf/2002.05700.pdf).






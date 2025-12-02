# Probabilistic Artificial Intelligence (PAI) - Course Projects

This repository contains the solutions for the projects of the **Probabilistic Artificial Intelligence** course, taught by Prof. Dr. Andreas Krause at **ETH Zürich**.

---

## 📝 Project Descriptions

The repository includes implementations for four main tasks of the course, each focusing on a different aspect of probabilistic artificial intelligence.

### 💨 Task 1: Gaussian Process Regression for Air Pollution Prediction

* **Objective:** To develop a model to predict the concentration of fine particulate matter (PM2.5) in locations without measurement stations, using Gaussian Process (GP) regression.
* **Challenges:**
    1.  **Model Selection:** Finding the most suitable kernel and its hyperparameters to accurately model the data.
    2.  **Scalability:** Addressing the computational complexity of GPs (which grows with O(n³)) on large datasets.
    3.  **Asymmetric Cost:** Avoiding underestimation of pollution concentration in candidate residential areas, where underestimation errors are penalized 50 times more than other errors.
* **Approach:** The solution implements a model which use multiple local GPs to face scalability. To handle the asymmetric cost function, multiple local GPs the final prediction is adjusted by adding a term proportional to the GP's standard deviation in the specific areas of interest.

---

### 🛰️ Task 2: Image Classification with Bayesian Neural Networks

* **Objective:** To classify satellite images into different land use categories. The training set consists of single-label images, while the test set includes multi-label images and images with atmospheric conditions like clouds or snow, which are not present in the training data.
* **Approach:** This task was solved using a **Bayesian Neural Network (BNN)**. Unlike standard neural networks that learn a single set of optimal weights, a BNN learns a probability distribution over its weights. This approach is ideal for quantifying **epistemic uncertainty**—the model's uncertainty about its own predictions.
* **Implementation:** Since computing the true posterior distribution over the weights is intractable, an approximation method called **Stochastic Weight Averaging Gaussian (SWAG)** was used. The **MultiSWAG** variant combines several independent SWAG models into a mixture of Gaussians for a more robust approximation.
* **Evaluation:** The ability to quantify uncertainty allows the model to abstain (predict -1) on out-of-distribution images, which is handled by a custom **cost function**. The model's performance was also evaluated

---

### 💊 Task 3: Hyperparameter Tuning with Bayesian Optimization

* **Objective:** To optimize the structural features of a drug candidate to maximize its bioavailability (measured by logP), while respecting a constraint on its synthesis difficulty (Synthetic Accessibility - SA).
* **Formalization:** The problem is defined as maximizing a noisy objective function $f(x)$ subject to an unknown constraint $v(x) < \kappa$.
* **Approach:** A **Constrained Bayesian Optimization** strategy was implemented.
    * Both the objective function and the constraint were modeled using **Gaussian Processes**.
    * The acquisition function used is the **Expected Improvement (EI)**, modified to account for the probability of satisfying the constraint.
    * To improve numerical stability and exploration in a noisy environment, techniques such as **LogEI** and a *plug-in* estimate for the current optimum were used.

---

### 🤖 Task 4: Implementing an Off-policy RL algorithm

* **Objective:** To implement an off-policy Reinforcement Learning (RL) algorithm to solve the **Cartpole** problem, which involves balancing a pole mounted on a controllable cart.
* **Algorithm:** The solution is based on **Soft Actor-Critic (SAC)**, a modern off-policy algorithm that maximizes a combination of expected reward and policy entropy. The entropy encourages greater exploration.
* **Architecture:** The agent consists of three neural networks:
    1.  **Actor:** Approximates the stochastic policy $\pi_{\theta}$, which maps states to actions.
    2.  **Critic:** Models two Q-functions ($Q_1, Q_2$) to mitigate overestimation bias.
    3.  **Critic Target:** Separate target networks, updated slowly (bootstrapping), to stabilize training.
* **Objective Function:** The agent maximizes the entropy-regularized expected reward, where the *temperature parameter* $\alpha$ balances the trade-off between exploitation and exploration. $\pi^{\*}=\arg\max_{\pi}\mathbb{E}_{\tau \sim \pi}$
    $\pi^{\*}=\arg\max_{\pi}\mathbb{E}_{\tau \sim \pi}[ \sum_{t=0}^{\infty} \gamma^t (R(x_t, a_t, x_{t+1}) + \alpha H(\pi(\cdot|x_t)))]$
  $$
\pi^{\ast}=\arg\max_{\pi}\mathbb{E}_{\tau \sim \pi}\left[\sum_{t=0}^{\infty} \gamma^t \left(R(x_t, a_t, x_{t+1}) + \alpha H(\pi(\cdot|x_t))\right)\right]
$$


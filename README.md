# WOMANIUM-project-2024
It is almost impossible to hear the word "climate" and avoid the word "change" echoing in one's mind right away. The paper "Climate Change and Flooding: Causes, Consequences, and Implications" by Hirabayashi et al. (2013) dicusses the impacts of climate change on precipitation patterns, snowmelt, and sea levels. These changes contribute to more frequent and severe flooding events. We will expand more on why floods are a major consequence of climate change, one that this project specifically sheds light on.
The aforementioned paper further emphasizes the need for variuos sorts of action to be taken in regards to the issue, including improved flood forecasting. Flood forecasting is in particular mentioned to state the fact that even in this very state of the project, quantum computing and AI can be applied: Forecasting floods in itself involves predicting floods which is a proccess of simulation/modelling/optimization problems. The paper "Quantum Computing and Weather Prediction" by Mete Atature et al. discusses the potential of quantum computing to revolutionize weather prediction by improving the accuracy of models and simulations. It touches on how quantum computing can handle complex atmospheric variables more efficiently than classical computers. But this is not where we apply quantum computing in this particular project. Another paper "The Impact of Climate Change on Extreme Floods" (Mallakpour & Villarini, 2015)  states that eventhough the impact of climate change on floods can vary significantly based on local geography, climate patterns, and land use, it provides evidence that climate change is contributing to more extreme flood event. 
By this point we have made it clear that floods are a major issue of the climate and like any other issue pertaining to the study of the world, Quantum Computing is a great tool on the way to predict and understand the nature of floods, through modeling and simulation. But the usage of quantum Computing doesn't fall short at that point, since I was curious enough to ask myself. Quantum computing can not only help us understand and predict weather patterns and therefor floods, but also actually come up with new ways, innovative solutions, to tackle the problem.
There are variuos approaches to confronting the challenge of floods, among which Flood Forecasting and early warning systems, urban planning and zoning and climate change mitigation. However, floods rise a contradictory aspect of climate change in my Middle-Eastern mind, where droughts are more of a concern. Approaching the problem from the other end of spectrum appeals to the author: What if we aim to balance precipitation rates so that there is no flood - and therefor no drought. This idea encourages a more expansive srudy of Weather Modification. Weather modification is a method that enables us to detect an incoming flood and spread out rainfall over a longer period or over a wider area, reducing the risk of flooding in one specific location. Cloud Seeding is a method applied to avoid both droughts and floods. However, research on cloud seeding as a means to prevent floods is relatively scarce, since most cloud seeding projects have historically focused on increasing precipitation in drought-prone areas rather than controlling or mitigating floods. One of the key challenges in using cloud seeding to prevent floods is the scale at which atmospheric forces operate. Cloud seeding can enhance rainfall from existing storm clouds, but it is not capable of controlling large-scale weather systems that often cause floods. For example, experts have pointed out that cloud seeding would have minimal impact on large convective systems, such as those responsible for severe flooding in the Middle East​. Overall, cloud seeding's application as a flood prevention tool is limited by the scale and unpredictability of atmospheric dynamics. Quantum computing has the potential to significantly advance the field of weather modification, including cloud seeding, particularly in addressing the challenges associated with preventing floods. 
By far, we have made it clear that this project focuses on preventing floods and accomplishing that goal through cloud seeding. Complex atmospheric modeling, optimization of seeding strategies, enhanced data processing and fusion, simulation of microphysical processes are examples of how quantum computing may aid us in paving the way for cloud seeding as a flood preventing tool. With its ability to process complex data at unprecedented speeds, quantum computing can enhance weather modification efforts in several ways:
	1. Improved Weather Modeling: Quantum computers can handle vast amounts of data and perform complex calculations much faster than classical computers, allowing for better predictions of cloud formation and precipitation patterns. Accurate models can optimize the timing and location of cloud seeding operations.
	2. Optimization Algorithms: For cloud seeding, this could mean optimizing the distribution and quantity of seeding agents to maximize effectiveness while minimizing environmental impact.
	3. Simulation and Prediction: Quantum simulations can model the interactions of seeding agents with cloud particles at a quantum level, providing insights into how these agents behave and affect precipitation. This could lead to more precise and effective cloud seeding techniques.
We take our attention to optimization algorithms. As a somewhat-beginner in quantum computing, I would like to apply my somewhat-basic knowledge at this point. Proposing an objective function for cloud seeding, this very last section of the project aims to solve it. To optimize the timing of cloud seeding using quantum annealing, we can create an objective function that balances maximizing the effectiveness of cloud seeding (e.g., precipitation) with minimizing environmental impact (e.g., unwanted side effects).
Objective Function: 
$$
\text{Objective} = \text{Maximize} \left( \sum_i \left( W_1 \cdot P(t_i) - W_2 \cdot E(t_i) \right) \cdot x_i \right)
$$

\begin{itemize}
    \item $t_i$ is the discretized time slot $i$
    \item $x_i$ is a binary variable indicating whether seeding is done at time $t_i$
    \item $W_1$ and $W_2$ are weights balancing the importance of precipitation and environmental impact
    \item $P(t_i)$ is the precipitation function at time $t_i$
    \item $E(t_i)$ is the environmental impact function at time $t_i$
\end{itemize}

Constraints:
1. Time
$$
\sum_i x_i = 1
$$
2. Penalty for Timing Constraint (added to QUBO):
$$
\lambda \left( \sum_i x_i - 1 \right)^2
$$

QUBO Formulation:
The objective function for the QUBO formulation becomes:
$$
H(x) = \sum_i \left( W_1 \cdot P(t_i) - W_2 \cdot E(t_i) \right) \cdot x_i + \lambda \left( \sum_i x_i - 1 \right)^2
$$

Using D-Wave:
Prepare the QUBO matrix (or Ising model equivalent).
Submit the QUBO problem to D-Wave via their API.

Implementation (in Python):

from dwave.system import DWaveSampler, EmbeddingComposite
from dimod import BinaryQuadraticModel

# Example weights and penalties (these would be calculated based on your specific model)
P = [0.1, 0.4, 0.3, 0.5]  # Example precipitation values for 4 time slots
E = [0.2, 0.1, 0.3, 0.1]  # Example environmental impact values for 4 time slots
W1, W2, λ = 1.0, 1.0, 10.0

# Construct the QUBO
Q = {}
for i in range(len(P)):
    Q[(i, i)] = W1 * P[i] - W2 * E[i] + λ * (1 - 2)

# Adding cross-terms for the constraint
for i in range(len(P)):
    for j in range(i + 1, len(P)):
        Q[(i, j)] = 2 * λ

# Solve the QUBO using D-Wave
sampler = EmbeddingComposite(DWaveSampler())
sampleset = sampler.sample_qubo(Q, num_reads=100)

# Extract the best solution
best_solution = sampleset.first.sample
selected_time_slot = [i for i in range(len(best_solution)) if best_solution[i] == 1]

In conclusion, the challenge of climate change, particularly the growing threat of floods, demands innovative and multifaceted solutions. Cloud seeding has emerged as one potential method to address this issue, yet the complexity of weather systems and the variability of outcomes highlight the need for more advanced tools. Through this project, the integration of quantum computing and artificial intelligence has been explored as a means to enhance our ability to predict, optimize, and refine these interventions. The application of D-Wave's quantum technology to an optimization problem demonstrates the potential of quantum computing to accelerate our understanding and improve our responses to climate-related challenges. As we continue to push the boundaries of these emerging technologies, their role in mitigating the impacts of climate change and safeguarding our future becomes increasingly vital.

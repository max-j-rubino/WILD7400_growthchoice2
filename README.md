# WILD7400_growthchoice
Class project coding a simple model relating to aspects of the landscape of fear framework in Ecology. The model will answer the question: Does an ecosystem that contains only the highest quality foods always support a large and stable population for a species that relies on this resource?

Key model attributes:
1. Size is related to an individual's willingness to search for high-quality food: larger individuals are more likely to search for better-quality food whereas smaller individuals may "make do" with lower quality food
2. Food quality is related to accessibility of that food resource: higher quality food is harder to find and will take longer to find/process compared to lower quality food sources

Other things that will need to be defined: 
1. Growth
2. Herbivore species reproduction
3. Food resource replication
4. Searching behavior


Edit with text editor like Text or BBEdit.


Model Overview/ ODD description

Purpose: This model will determine via simulation if an ecosystem that contains only the highest quality foods will always support a large and stable population for a species that relies on said food.

Overview:

Entities, state variables, and scales: The model has two entities:  an animal (say elk) and land patches. Patches are all of equal size (say 200 square meters) and described by one discreet variable (patch quality: zero, low, med, high). The animal is described by three discreet variables: sex, growth trajectory, and age. Animal size is derived from age via a stochastic growth process dependent on the animal size and growth trajectory (slow/med/fast), and the quality of patches they forage in. Growth trajectories are determined by two alleles. Fast growers are FF, Medium are FS, and Slow are SS. The foraging range is dependent on an individual animal's size. We assume that an animal agent will leave patches once optimal foraging has passed. We assume that animals know the probability of encountering each patch quality, and can assess patch quality with 100% accuracy. Patch quality will decrease after foraging and increase based on the time between foraging events. We assume that all patches in the 150x150 grid of patches are accessible to all animals (all patches within home range).  Larger animals will have higher metabolic rates, and require more food thus need to encounter higher quality patches. Every patch explored by animals without foraging will decrease survival. Smaller animals can sustain themselves without relying on higher-quality patches. The model will run in weekly time steps for 50 years. 

Processes: The probability of an animal foraging in a patch is dependent on the animal's size and the probability of encountering high-quality patches. Larger animals are more likely to ignore lower-quality patches. When animals cross patches they will incur a metabolic debt that will lower their survival until they forage. Fast growers will also have a greater metabolic demand than slow growers. After foraging patch quality will be reduced by one level (e.g. High to medium). Animals can only forage in one patch per period. Patches that are not forged can go up a quality level with a defined probability. Patches of zero quality cannot be foraged and will take the longest to replenish. We assume that all animals that encounter a high-quality patch will forage. We assume no predation happens, and that mortality happens during the transition between time steps. Transitions from juvenile to adult will happen once a year based on the animal's size. Reproduction will happen immediately after, and animals that have a high metabolic debt will be less likely to mature. Each individual in a successful reproduction will contribute one of its two growth alleles at random. 

Design Concepts: 

Basic principles: The model explores concepts related to the landscape of fear. Specifically, an animal's willingness to travel further to reach higher-quality foods is dependent on its size. We also assume that traveling between patches involves some sort of cost that affects survival. In our example this is metabolic, but it can be expanded to include other costs such as the risk of predation. When there are more high-quality patches, animals are more likely to ignore low-quality patches. 

Emergence: The results of this model are the quality of patches, abundance of animals, population size and age structure, growth trajectories, sex ratio, and average size at age. Decreased size at age demonstrates a selective pressure towards smaller individuals who do not have to travel as far to reach foraging patches of needed quality.

Adaptation: No direct objective seeking exists in this model. Indirect objective seeking exists that animals of larger size are more likely to ignore patches and incur a metabolic debt. However, these animals are more likely to reproduce. Whether an animal forages in a patch or not is purely dependent on its size, they cannot learn in this model and there is no heritable component.

Objectives: There is a tradeoff between agent size, foraging habits, growth trajectory, and reproduction. Smaller animals that forage in low-quality patches are more likely to survive but less likely to reproduce. Larger animals have to travel further between patches incurring a metabolic debt, but they are more likely to reproduce. Fast growers will have lower survival when patch quality is low but will reach maturity quicker. Thus the measures of agent objectives are determined by growth trajectories and size. 

Learning: This model does not allow animals to make adaptive changes over time. We could add a component where the ability to recognize the probability of encountering high-quality patches is learned. 

Sensing: We assume that animals are able to access patch quality with 100% accuracy and are aware of the probability of encountering high-quality patches. 

Interactions: Animals in this model interact when two animals are in the same patch at the same time. When this happens we assume the larger animal is able to defend the patch, which incurs some metabolic cost. The smaller animal will have to travel to another patch, incurring metabolic costs as well. 

Stochasticity: Stochasticity in this model results in animals' fates (breeding, foraging, crossing patches, etc.) being drawn from a categorical distribution (or since r does not have a categorical distribution function a multinomial with a size of 1). Probabilities from this distribution are dependent on size, which is derived from growth trajectories and metabolic activity. There is some stochasticity built in these by using a normal distribution centered around the mean rate for an animal of that size, with some pre-defined standard deviation (say 0.005). Stochasticity is used to represent processes that are impossible to represent mechanically. These processes are either poorly understood or limited by computing power. 

Collectives: Collectives are not represented in this model.

Observation: Outputs from the model needed to observe system-level behaviors are: growth trajectory ratio, sex ratio, size and age structure, mean age at maturity, average patch quality, and population abundance.

Initialization: We assume equal sex ratios, growth trajectory ratio, and size & age structure ratios at initialization. We will start with a population size of 50 individuals, and all patches start as high quality. We will use a burn-in period until equilibrium is reached where the landscape of fear attributes does not apply. We will run a control for an additional 50 years (in weekly time steps) where the landscape of fear attributes is not applied. We will also run a treatment model where the landscape of fear attributes is applied. 

Input data: There will be no input data other than that described in the initialization above

Subroutines: The model will have several subroutines. The smallest is the choice to forage at an individual patch or not. There will be a cap on the number of patches an animal can visit within each timestep that is determined by its size. The animals' metabolic debt, and the number of patches ignored since last foraging will increase the probability of an individual foraging at a given patch. Mortality can only happen during the transition between weeks, as does growth. This weekly loop will be inside of a yearly loop. Maturing, mate pairing, then reproduction happens during the transition between years. Males can father multiple offspring, and the probability of them doing so is positively correlated with their size. Larger females are also more likely to reproduce, however, they can only produce one offspring. 

# JFT_ADS_Thesis
**Including python file and notebook used to investigate the impact of contact on the homogeneity of language features. Script has been run using the notebook, hence it is advised to run the notebook.**

To run the Notebook, no changes need to be made. The Python file requires the below functions to be used by the user.

When running the Notebook, the script will run as completed in the research. 
The python file does not include specific experiments or visualisation, but instead offers a method for a researcher to call the functions themselves with certain parameters.
A guide to these functions is presented below:
-------------------------------------------------------------------------------------------------------------------------------------
Packages:
- pandas==1.5.3
- mesa==1.2.1
- random2=1.0.1
- numpy==1.21.0
- more-itertools==9.1.0
- scipy==1.10.1
- matplotlib==3.7.1
- regex
- math
- copy
-------------------------------------------------------------------------------------------------------------------------------------

# Running the model:
- The model must be defined with the following input parameters:

```
model = Language_Model(number_of_agents=,                      # Initial number of languages
                max_number_of_states=,                   # Max number of states (Min is always 2, 8 is the max allowed by the script)
                number_of_features=,                    # Number of features per language
                grid_size=,                             # Size of grid world. If == 20: Grid == 20x20
                shortcut_pct=,                         # Percentage of cells that will be a shortcut
                interaction_likelihood=,               # Chance to have contact with neighbour
                inheritance_rate_lower=,              # Lower range of inheritance features (I suggest min 50%)
                inheritance_rate_higher=,             # Max percentage of inheritance (Atleast 1 less than total feature count)
                birth_likelihood=,                     # Chance to have a child (if they do not interact)
                child_relocation_likelihood=, # Chance child is born in a neighbouring cell
                birthing_stop=)                            # Time-step that a child stops being created

``` 

- The model is ran with the following function(```number_of_iterations``` to be replaced with the required number of time-steps):

```df, inheritance_dict, inherit_states, s_v = model.run_simulation(number_of_iterations)```

- The model function returns:
    - ```df```: this is the dataframe including all time-steps for all agents
    - ```inheritance_dict```: a dictionary storing data regarding inherited states (who is the parent language and what percentage of states were inherited by the child)
    - ```inheritance_states```: a dictionary listing which states a child inherited from a parent, and which were caused by universal preference
    - ```state_values```: a dictionary of the potential feature states of each feature
   
-------------------------------------------------------------------------------------------------------------------------------------
# Identifying causes of each feature:
- The ```effect_measures``` function takes the ```df```, ```inheritance_dict```, and ```inheritance_states``` as inputs from the previous section.
- The function returns:
  - ```causes_dict```: a dictionary converted to a dataframe which shows the causes of each final feature state per language (either universal preference, contact, or inheritance)
  - ```final_step_df```: a dataframe including only the final feature states for each language (can be similarly called using the return_final_state function, using df as the input)
  - ```universal initial```: a dictionary indicating the preference of the most favoured feature states in the initialisation (e.g., if Feature_1's most common state is "A", and 6/10 agents have "A" for this feature, the Universal Preference for Feature_1 = 60%)

-------------------------------------------------------------------------------------------------------------------------------------
# Running entropy experiments:
- The experiment is called with the function below. The parameters must be defined by the researcher using the same constraints as running the model
- The benchmark experiment is included as an example
- The function returns a dictionary which includes (per level of contact) the entropy, normalized_entropy, and the causes of all feature states within the experiment.
  The experiment will always run 10 iterations of the model, the values of the dictionary are mean values across these 10 iteations   

```
experiment(number_of_agents=10,
                max_number_of_states=4,
                number_of_features=10,
                grid_size=8,
                shortcut_pct=0.2,
                inheritance_rate_lower=0.50,
                inheritance_rate_higher=0.90,
                birth_likelihood=0.05,
                child_relocation_likelihood=0.25,
                birthing_stop = 21,
                number_of_iterations = 100)
```

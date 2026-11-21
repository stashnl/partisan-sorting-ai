# Realistic polarisation sim

Does online contact sort society into two party cultures on a *realistic* social network, and what changes when everyone also talks to an AI chatbot?

1. **Build a realistic social network** with Băbeanu's feature-driven graph model: heavy-tailed degrees, high clustering, communities, short paths.
2. **Validate Törnberg (2022)** on it: does contact with strangers still drive partisan sorting?
3. **Extend it with AI** (idea from Balázs Vedres): add a chatbot as a conversation partner, in two evidence-backed forms.

## Notebooks

| | |
|---|---|
| [`01_feature_driven_network.ipynb`](01_feature_driven_network.ipynb) | Building the realistic social network |
| [`02_tornberg_sorting.ipynb`](02_tornberg_sorting.ipynb) | Törnberg's sorting model on it, plus the chatbot extension |

Explainers: [the network](docs/01_realistic_network.md) · [the AI extension](docs/02_ai_chatbots.md)

## Key results

**Sorting on a realistic network.** More contact with strangers (γ) still means more sorting, as Törnberg found. Communities slow it down, and the mean number of contacts matters most.

![Sorting vs gamma](figures/02_sorting_vs_gamma.png)

Shape = party, colour = shared culture. With few strangers, each party splits into local cultures; with many, each party converges on one culture across the network.

![End states](figures/02b_end_states.png)

**With a chatbot.** At a realistic 20–40% contact with strangers, a mirror chatbot in 1% of conversations lifts sorting from ≈ 0.3 to ≈ 0.8. An aggregator in 3% of conversations merges everyone into one culture.

![Chatbot results](figures/03_llm_extension.png)

## The AI extension in brief

In Törnberg's model a person repeatedly picks someone to talk to, preferring people like themselves, and copies one of their traits. The extension adds a chatbot: with probability α a conversation is with the chatbot instead of a person, and the person copies one trait from its answer. The chatbot is shared by everyone, has no homophily filter (people don't treat their assistant as an outsider), and retrains on the population every round. It comes in two forms, each based on documented LLM behaviour:

- **Aggregator:** answers with the majority view of the whole population. LLMs pull writing and ideas towards the typical (Doshi & Hauser 2024; Padmakumar & He 2024).
- **Mirror:** answers with the majority view of *the user's own party*. LLMs tend to tell users what they already believe (Perez et al. 2023; Sharma et al. 2024).

Both are deliberately extreme versions of real tendencies, so the effects are upper bounds. Full reasoning, sources and limitations are in the [AI explainer](docs/02_ai_chatbots.md).

## Replicating the results

1. **Get the code and the environment** (needs [conda](https://docs.conda.io/en/latest/miniconda.html); package versions are pinned in `environment.yml`):

   ```bash
   git clone https://github.com/stashnl/realistic-polarisation-sim.git
   cd realistic-polarisation-sim
   conda env create -f environment.yml
   conda activate socnet-polar
   python -m ipykernel install --user --name socnet-polar --display-name "Python (socnet-polar)"
   ```

2. **Run notebook 1** in Jupyter (`jupyter lab`), with the kernel *Python (socnet-polar)*. It builds the network and saves it to `data/` (a few minutes).
3. **Run notebook 2.** It reads the network from `data/` and runs all simulations (about 10 minutes on an 8-core laptop; it uses all cores, and the first run spends a little extra time compiling).

All random numbers are seeded, so the runs reproduce the saved figures and tables. The outputs of both notebooks are already in `data/` and `figures/`, so notebook 2 can also be run on its own.

To run a notebook from the command line instead:

```bash
jupyter nbconvert --to notebook --execute --inplace 01_feature_driven_network.ipynb
```

## Background and AI assistance

This project began in A. I. Băbeanu's *Structures of Networks* course at Leiden University and was revived as coursework at CEU in 2026.

**The code was written with AI coding assistance.**

## Credits

- **Petter Törnberg**, for the partisan-sorting model this project re-implements and extends: Törnberg, P. (2022). How digital media drive affective polarization through partisan sorting. *PNAS*, 119(42), e2207159119. https://doi.org/10.1073/pnas.2207159119
- **A. I. Băbeanu**, for the feature-driven random graph model used to build the network, from his *Structures of Networks* course.
- **Balázs Vedres**, for the idea of adding AI to the model.

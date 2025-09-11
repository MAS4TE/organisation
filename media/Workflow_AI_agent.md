# AI agents

<img width="1920" height="1080" alt="AI_Agents_1" src="https://github.com/user-attachments/assets/6bfeb480-293b-4208-a71d-14b452e16193" />

https://www.canva.com/design/DAGyptuNCTg/5m9gBuTWbCBhh3NP3jGk9A/edit?utm_content=DAGyptuNCTg&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

## Finished Steps
- The code for the Mistral API with credits is implemented in ASSUME.
- An example to run it locally with LM studio is also implemented.
- Ensure correct output format for bid from LLM agent (build safety net with default value for when the LLM does not work, or gives wrong output).

## Current Agent Architecture
- Class: LLMModelAPI
  - Loads the API model
  - The LLM agent in ASSUME sends their prompt to this class, so that the language model can run the prompt.
  - This will in the future be replaced by a communication protocol. 

- LLM in mas4te_bidding_strategy
  - a bid has to be made for a specific timeframe
  - The LLM agent receives a prompt and the optimization framework.
  - The optimization framework receives the predictions for price, energy demand and solar generation for a specific unit.
    It then calculates price recommendations for specific storage volumes based on its inputs. 
    This returns the monetary worth of the storage volume to the unit operator.
  - The LLM agent receives this optimization framework output, in order to reason about the possibilities of price and volume.
  - Currently, the agent chooses a price and volume based on the output of the optimization framework. However, it struggles to understand what this optimization framework is, as it sees it as profit, not as costs. 
  - The output should be in the format of a JSON object with 'volume', 'price', and 'reasoning' as keys.
  - This is then combined with the timeframe as a bid onto the ASSUME framework. 
 

## Next Steps
- Incorporate tools (toolbox) to combine all input signals into one larger system that can reason and act. 
- Incorporate short history of market, to see how the AI agent reasons about this. 

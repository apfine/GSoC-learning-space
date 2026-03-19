# Review Note: LLM Prisoner's Dilemma — when agents reason about trust, not just strategy

Short review note for Mesa Examples PR #378, covering the overall value of the contribution, implementation strengths, possible improvements, and a few design questions for future exploration.

PR link: [https://github.com/mesa/mesa-examples/pull/378](https://github.com/mesa/mesa-examples/pull/378)

Review link: [https://github.com/mesa/mesa-examples/pull/378#issuecomment-4090096804](https://github.com/mesa/mesa-examples/pull/378#issuecomment-4090096804)

## Summary

This is a valuable and interesting addition to the Mesa Examples repository. By using a language model to drive chain-of-thought style reasoning instead of relying only on classical game theory, the model captures more complex parts of social interaction such as trust, forgiveness, and reputation. Through the review process, the implementation has also become much more robust, especially around simultaneous decision-making and output handling.

## What stands out

**Concurrent execution for simultaneity**: Moving the LLM API calls to `ThreadPoolExecutor` is a strong design choice. It helps prevent the Solara UI from freezing and better preserves the intended simultaneous nature of the Prisoner's Dilemma by avoiding unintended information leakage between agents.

**Robust output parsing**: Replacing substring matching with regex-based parsing around an `ACTION` tag is a solid improvement. It makes the system more resilient when LLM responses vary in wording or format.

**Mesa standard alignment**: Using Mesa's built-in time attributes instead of a custom round counter is a good cleanup. It aligns the model better with standard Mesa patterns and improves compatibility with tools like `DataCollector`.

**Clean UI integration**: The Solara dashboard makes the simulation easier to inspect by surfacing metrics such as cooperation rate and collective welfare in a clear way.

## Possible improvements

**Inline documentation**: The code would benefit from a few targeted comments, especially around the `ThreadPoolExecutor` implementation and prompt/output parsing flow, so future contributors can understand the reasoning more quickly.

**Prompt abstraction**: The `SYSTEM_PROMPT` is currently embedded in the agent logic. Moving it into a separate file would make it easier for researchers to experiment with agent personalities and behavior.

**Token/cost tracking**: Since LLM calls can become expensive, adding token or cost tracking would help users manage API usage when scaling the simulation.

## Design questions

**Memory decay**: As the simulation runs, the internal context history may keep growing. Should there be a limit on how many rounds agents remember, both for efficiency and to better mimic bounded human memory?

**Cheap talk mechanics**: Right now, agents appear to observe past actions only. Would the dynamics change meaningfully if agents could communicate before making decisions?
# Prerequisites

- Leveraging the power of Open Source and Creative Commons, because knowledge should always be free
- Create structure for the learning text and the learning katas
- hence, make them machine readable and i.e. transformable

# Architecture

```mermaid
graph TD;
  subgraph Repository
    c1(Card)
    c2(Card)
    k1(Kata)
    k2(Kata)
    k3(Kata)
    k4(Kata)
  end
```

A repo consists of

- cards: bite-sized content for the learner
- katas: katas for self-improving on the topic

Every element is one file, based on

- YAML code for configuration, meta data
- Markdown language for content: easy to learn, portable, extendable, transformable
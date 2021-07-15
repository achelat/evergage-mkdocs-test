---
path: '/data-model/datasets'
title: 'Datasets'
tags: ['dataset']
---

Datasets represent an instance of Interaction Studio for use in personalization. User profiles, campaign configurations, and other features of Interaction Studio are scoped to a dataset. This permits the use of different datasets on different sites, geographies, or different deployment environments. For example, it is common to split datasets out between QA and production, but it may also be useful to have different datasets for your French and German sites.

Datasets are placed within Identity Groups for the purpose of coordinating user identities between environments. This can help to make sure you're not emailing the same user from both the French and German environments while still letting each dataset coordinate that user's personalization.
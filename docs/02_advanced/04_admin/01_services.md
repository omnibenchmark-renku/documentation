## Services dependencies

Several components, mainly omnibenchmark python, query other components (APIs, triplestore) on runtime.

- Renku API
    - omnibenchmark python
- Gitlab API
    - omnibenchmark python
    - gitlab runners
- Triplestore
    - omnibenchmark python (query)
    - omnibenchmark python (population)
    - anywhere (population, from within the CI/CD job; update token needed)
    - (not yet implemented) git/renku hooks
- Metric results
    - a cron job from the machine serving bettr deployments 
    
## Tips

You can run renku projects in `renku stealth mode` disabling the `https://renkulab.io/webhooks/events` hook within your gitlab project by browsing `Settings -> Webhooks`.

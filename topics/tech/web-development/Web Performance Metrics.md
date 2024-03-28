# Web Performance Metrics

## [User-centric performance metrics](https://web.dev/articles/user-centric-performance-metrics)

### [Important metrics to measure](https://web.dev/articles/user-centric-performance-metrics#important_metrics_to_measure)

| Metric | Description |
| --- | --- |
| First Contentful Paint (FCP) | measures the time from when the page starts loading to when any part of the page's content is rendered on the screen. |
| Largest Contentful Paint (LCP) | measures the time from when the page starts loading to when the largest text block or image element is rendered on the screen. |
| Interaction to Next Paint (INP) | measures the latency of every tap, click, or keyboard interaction made with the page, and—based on the number of interactions—selects the worst interaction latency of the page (or close to the highest) as a single, representative value to describe a page's overall responsiveness. |
| Total Blocking Time (TBT) | measures the total amount of time between FCP and TTI where the main thread was blocked for long enough to prevent input responsiveness. |
| Cumulative Layout Shift (CLS) | measures the time from when the page starts loading to when the largest text block or image element is rendered on the screen. |
| Time to First Byte (TTFB) | Measures the time it takes for the network to respond to a user request with the first byte of a resource.

```mermaid
mindmap
    root(User-centric performance metrics)
        Defining metrics
            [Key questions]
                ("Is it happening?")
                ("Is it useful?")
                ("Is it usable?")
                ("Is it delightful?")
        How metrics are measured
            [**In the lab**: using tools to simulate a page load in a consistent, controlled environment]
                (Essential when developing new features)
                (Used before features are released in production, when gathering real performance data is not possible)
                (Best way to prevent performance regressions)
            [**In the field**: on real users actually loading and interacting with the page]
                (Not necessarily reflective of user experiences in the wild)
                (Varies dramatically based on network conditions and user interactions)
                (Page loading may not be deterministic; performance may be vastly different from user to user)
        Types of metrics
            (Perceived load speed)
            (Load responsiveness)
            (Runtime responsiveness)
            (Visual stability)
            (Smothness)
        Important metrics to measure
            ("Largest Contentful Paint (LCP)")
            ("Cumulative Layout Shift (CLS)")
            ("Interaction to Next Paint (INP)")
            ("Time to First Byte (TTFB)")
            ("First Contentful Paint (FCP)")
            ("Total Blocking Time (TBT)")
        Custom metrics
            (User Timing API)
            (Long Tasks API)
            (Element Timing API)
            (Navigation Timing API)
            (Resource Timing API)
            (Server timing)
```

## References

* [**web.dev**](https://web.dev/)
    * [Metrics](https://web.dev/explore/metrics)
    * [User-Centric Performance Metrics](https://web.dev/articles/user-centric-performance-metrics)
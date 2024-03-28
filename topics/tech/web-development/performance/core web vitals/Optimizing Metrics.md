# Optimizing Metrics

This document compares the important metrics to measure and the ways to optimize them:

* First Contentful Paint (FCP)
* Largest Contentful Paint (LCP)
* Interaction to Next Paint (INP)
* Total Blocking Time (TBT)
* Cumulative Layout Shift (CLS)
* Time to First Byte (TTFB)

<table>
    <caption>Measuring & improving user-centric performance metrics</caption>
    <thead>
        <tr>
            <th rowspan="2">Metric</th>
            <th rowspan="2">Score ranges</th>
            <th colspan="2">How to measure</th>
            <th rowspan="2">How to improve</th>
        </tr>
        <tr>
            <th>Field tools</th>
            <th>Lab tools</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>First Contentful Paint (FCP)</th>
            <td>
                <ul>
                    <li>Good: < 1.8 seconds</li>
                    <li>Needs improvement: 1.8 ~ 3.0 seconds</li>
                    <li>Poor: > 3.0 seconds</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>PageSpeed Insights</li>
                    <li>Chrome User Experience Report</li>
                    <li>Search Console (Speed Report)</li>
                    <li><code>web-vitals</code> JavaScript library</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Lighthouse</li>
                    <li>Chrome DevTools</li>
                    <li>PageSpeed Insights</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Eliminate render-blocking resources</li>
                    <li>Minify CSS</li>
                    <li>Remove unused CSS</li>
                    <li>Remove unused JavaScript</li>
                    <li>Preconnect to required origins</li>
                    <li>Reduce server response times (TTFB)</li>
                    <li>Avoid multiple page redirects</li>
                    <li>Preload key requests</li>
                    <li>Avoid enormous network payloads</li>
                    <li>Serve static assets with an efficient cache policy</li>
                    <li>Avoid an excessive DOM size</li>
                    <li>Minimize critical request depth</li>
                    <li>Ensure text remains visible during webfont load</li>
                    <li>Keep request counts low and transfer sizes small</li>
                </ul>
            </td>
        </tr>
        <tr>
            <th>Largest Contentful Paint (LCP)</th>
            <td>
                <ul>
                    <li>Good: < 2.5 seconds</li>
                    <li>Needs improvement: 2.5 ~ 4.0 seconds</li>
                    <li>Poor: > 4.0 seconds</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>PageSpeed Insights</li>
                    <li>Chrome User Experience Report</li>
                    <li>Search Console (Speed Report)</li>
                    <li><code>web-vitals</code> JavaScript library</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Lighthouse</li>
                    <li>Chrome DevTools</li>
                    <li>PageSpeed Insights</li>
                    <li>WebPageTest</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Eliminate resource load delay</li>
                    <li>Eliminate element render delay</li>
                    <li>Reduce resource load time</li>
                    <li>Reduce time to first byte</li>
                </ul>
            </td>
        </tr>
        <tr>
            <th>Interaction to Next Paint (INP)</th>
            <td>
                <ul>
                    <li>Good: < 200 milliseconds</li>
                    <li>Needs improvement: 200 ~ 500 milliseconds</li>
                    <li>Poor: > 500 milliseconds</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Real User Monitoring (RUM)</li>
                    <li>CrUX in PageSpeed Insights</li>
                    <li>Data on other core web vitals</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Interactions following common user flows</li>
                    <li>Interaction during page load, when the main thread is usually the busiest</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Identify and reduce input delay</li>
                    <li>Optimize event callbacks</li>
                    <li>Minimize presentation delay</li>
                </ul>
            </td>
        </tr>
        <tr>
            <th>Total Blocking Time (TBT)</th>
            <td>
                <ul>
                    <li>Good: < 200 milliseconds on average hardware</li>
                </ul>
            </td>
            <td>
            </td>
            <td>
                <ul>
                    <li>Lighthouse</li>
                    <li>WebPageTest</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Reduce the impact of third-party code</li>
                    <li>Reduce JavaScript execution time</li>
                    <li>Minimize main thread work</li>
                    <li>Keep request counts low and transfer sizes small</li>
                </ul>
            </td>
        </tr>
        <tr>
            <th>Cumulative Layout Shift (CLS)</th>
            <td>
                <ul>
                    <li>Good: < 0.1</li>
                    <li>Needs improvement: 0.1 ~ 0.25</li>
                    <li>Poor: > 0.25</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>PageSpeed Insights</li>
                    <li>Chrome User Experience Report</li>
                    <li>Search Console (Speed Report)</li>
                    <li><code>web-vitals</code> JavaScript library</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Chrome DevTools</li>
                    <li>Lighthouse</li>
                    <li>PageSpeed Insights</li>
                    <li>WebPageTest</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Understand the common causes of layout shifts:</li>
                    <ul>
                        <li>Images without dimensions</li>
                        <li>Ads, embeds, and other late-loaded content</li>
                        <li>Animations</li>
                        <li>Web fonts</li>
                    </ul>
                    <li>Make your pages eligible for the bfcache</li>
                </ul>
            </td>
        </tr>
        <tr>
            <th>Time to First Byte (TTFB)</th>
            <td>
                <ul>
                    <li>Good: < 800 milliseconds</li>
                    <li>Needs improvement: 800 ~ 1800 milliseconds</li>
                    <li>Poor: > 800 milliseconds</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Chrome User Experience Report</li>
                    <li><code>web-vitals</code> JavaScript library</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>In the network panel of Chrome's DevTools</li>
                    <li>WebPage Test</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Reduce the impact of third-party code</li>
                    <li>Reduce JavaScript execution time</li>
                    <li>Minimize main thread work</li>
                    <li>Keep request counts low and transfer sizes small</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

## References

* Important metrics to measure:
    * [Largest Contentful Paint (LCP)](https://web.dev/articles/lcp)
    * [Cumulative Layout Shift (CLS)](https://web.dev/articles/cls)
    * [Interaction to Next Paint (INP)](https://web.dev/articles/inp)
    * [Time to First Byte (TTFB)](https://web.dev/articles/ttfb)
    * [First Contentful Paint (FCP)](https://web.dev/articles/fcp)
    * [Total Blocking Time](https://web.dev/articles/tbt)
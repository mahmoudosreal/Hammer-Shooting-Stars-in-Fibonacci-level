# Hammer & Shooting Stars Fibo Indicator

This Pine Script™ code identifies Hammer and Shooting Star candlestick patterns using Fibonacci levels. It plots visual signals on the chart and triggers alerts when these patterns are detected.

## License

This Pine Script™ code is subject to the terms of the Mozilla Public License 2.0. See [Mozilla Public License 2.0](https://mozilla.org/MPL/2.0/) for more details.
© mahmoudos

## Indicator Code

```pinescript
//@version=5
indicator("Hammer & Shooting Stars Fibo", overlay = true)

// User Inputs for customization
// fibLevel is the Fibonacci level used in calculations, default is 0.333
fibLevel = input.float(title = "Fib Level", defval = 0.333)
// colorFilter determines if the color of the candle should be considered in the pattern validation, default is false
colorFilter = input.bool(title = "Color Must Respect The Rules", defval = false)

// Calculate Fibonacci levels for the current candle
// bullFib is the Fibonacci level for a bullish (upward) candle
bullFib = ((low - high) * fibLevel) + high
// bearFib is the Fibonacci level for a bearish (downward) candle
bearFib = ((high - low) * fibLevel) + low

// Determine the body of the candle
// lowestBody identifies the lower price of the candle body (either close or open)
lowestBody = close < open ? close : open
// highestBody identifies the higher price of the candle body (either close or open)
highestBody = close > open ? close : open

// Determine if we have a valid Hammer or Shooting Star pattern
// A hammer candle is identified if the lowestBody is above the calculated bullFib level and if colorFilter is true, the candle must be bullish (close > open)
hammerCandle = lowestBody >= bullFib and (not colorFilter or close > open)
// A shooting star candle is identified if the highestBody is below the calculated bearFib level and if colorFilter is true, the candle must be bearish (close < open)
starCandle = highestBody <= bearFib and (not colorFilter or close < open)

// Plot visual signals on the chart
// Plot an upward arrow below the bar if a hammer candle is identified
plotshape(series = hammerCandle, style = shape.arrowup, location = location.belowbar, color = color.green)
// Plot a downward arrow above the bar if a shooting star candle is identified
plotshape(series = starCandle, style = shape.arrowdown, location = location.abovebar, color = color.red)

// Trigger alerts for identified patterns
// An alert is triggered if either a hammer or shooting star pattern is identified
alertcondition(condition = hammerCandle or starCandle, title = "Hammer or Shooting Star Alert", message = "{{ticker}}")
```
## Example Chart

Below is an example chart showing the Hammer & Shooting Stars Fibo Indicator in action:

![Example Chart](https://lh3.googleusercontent.com/fife/ALs6j_H18D0RSIMCDcyCsfLY4N8Azw4-9zkUXCmpU0xlB_1E5q4nLqtNaRxb7N3d4CPIMtXk7KAcSf1600FDz209svhsCpbtgeAJM-YcWLowzmiQvO7GazedZMetvGt3A5jbztZpD75_XxQmYg9izSuxCvX9O7IwSVqtPkqb5yj-EDH4mfk9YLx6r4FieAZk-GWIT734m7KR93V5kXRTH4YmfoQ-PwoEVm3alC7jrgNZJ5a7NfDuTrni02jnqrHEvnIrTGhX5G2EMvjlJhVhg14v_C175F-kteRC4tbYSDPZiuY9DSJrpml6iosLG8Z0DwWcCeGXAZU7votvRRnnUEqsuYHRmvDpI9Bsp6fI0ltPfoD4Qc5qWhBsfSAj-gqMSJlfUon_kT6BkaQAMDdraV5PGGthzjJv2vVA8cs0O04oOdbzwWpndDFG9Q3OlvdkRAwE2CZjuCf71KvPjgNRMInFh7UE_Tkm89lOo_9iCvKO2rEujK0adTYb1ytZoMfnDqZZOr0Hx2AETp5_CyFif4xipzpBbspVupnqIr9AF2bNW2XRvqmt9TCRmg4_nQTob4ePvAqfxle3QfhVJLJNYwSBpvEWcEjqO0sQKIBiqoQWcnw_ky1SWJramg_rGQ8CAjdBdOldQCrX-4pcGQQcCUE6h7e5CjV6B5jBG6V9_Y9kAx3xGkoTUTcGbBJwTbXzMRDgc9xhIq3B-GwceiIkO-tgU9L9dM3AqQ6UVDT5Nn00a548tq6JvrOg8PZhGuJ2TVNri7HXjT6s_ngi3gW5B8_jy8XJg67XIGCLMHZcXXWYWPUlW2KBOUWYFwTCbB59lHew6S6cXol6j2iwr0KD-wctqHEHUr-MX2uN-fTN7e6voSFsmWWtcY9sl1CpAyszXPFOOyBZY1ZxQt-AHirEAk6btyhzf7hHUd5H-s6dN5mXWOyhmIUG8V35WbI3C42rq9RJ4h8057dBd_orUiecTcndryIdBaIjpcEQimwWvITGf8MSRcHZRJ-4kU3Q8-AHPusTalXzMa-5yMa9Hc39WKxlef-NkB-44JQBgxENjw6uTru2h41MGNvkkxMgjNJ6yONR79pLJpeUe2UEiUaQu9veDv_EXO0HWoqBUIWZI0Rhv9qc5xpxW9p27vy5RMeLb0kiNAtAbpYiuyy5jvjChKhwCzKNQ6MY2oRK8zV-sZKrCZGXOClq5eGYCjBluEn-AjK4Mm3sm2F1WE8MPhq48Fb6JpL02wYAqf3b-ulbLTfErTAA4eZrliFTmEU5QRQHwEyBbjdnjIDQboCvT2fga9SH09J-YL3FqEoZbVhSYr0k8VEVU9N6ggvuVqTGd29MSYQoZBm0e77cJtktZqOYTkQ0oAtYAmeR_ptaOrajWUYc0FS-7SzkiV-Lsa5TZ4Y4T_350_FtYH62agirqv4wKz_2hfJaSzCtVldCS1lvgjHdMc-t657EumWKF0vXKCayOCYQKGSeXLmdjgPWEgN6Llh4pgPM785fcyAktWkPRS7AS-W-t1OCM4ViZcbInlSI7ykj0UZU0VSH_WFGc3dUBWH2NKrwtg1ZPTjLj4ptqVxUkefuQA2QKAENWjGzGZFQanojaCXCPv6Iov0TXPseKpPHv-fbLQFbURrWVXjBvURPgMEckb1DfZr6rmJPtCeL-Qt_6AQWulxjmJP42cxBWAJVch2iPP9EIgT6XN_m6zTeJHdh5XrQT5-Pch9318RVtDTgG79bgECWJSepyRqFQ5OEza039fTD-WX14ti-8KAPCFG2HjPd1iJxn-vTDV10i5_HobVYe7paYkb2CoPQ_uDh6J3xMiu1n_UTktJU2b5lBrL9QijXtX0UvJV4sQQADVyEfv2gGdOvrlIJ)

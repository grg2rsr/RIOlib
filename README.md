# RIOlib
A small library for programmatically reading, writing and manipulating RIO sequence `.sqc` files.

## Description
cRIO is a general purpose TTL communication interface developed by Stefanie Neupert at the University of Konstanz, lab of Christoph Kleineidam. This library provides programmatical access to freely generate different TTL pulse patterns via a object oriented interface based on the same logic as the cRIO (a sequence holds patterns, a pattern consists of pulses). RIOlib provides functions to read existing `.sqc` files and manipulate them, as well as to generate them from scratch.

## Examples / Usage

### Permutation of existing patterns
An existing `.sqc` file is read and the containing patterns are pseudorandomized 3 times

```python
S = read_sqc('test.sqc')
S_perm, S_labels, S_indices = randomize_patterns(S.Patterns, nReps=3, seq_name='3x permuted', pseudorandom=True)
S_perm.write_sqc('test_perm.sqc')
```
`S_labels` now contains the names in the new order

### Random state changes
Random changes or random triggering is impossible to achieve via the UI. Therefore, the library contains functions to convert a vector of 0 and 1 into TTL states, as well as convenience functions that directly yield a RIO pattern. Here, the times between two state changes are drawn from an exponential distribution with a scale parameter 'tau'.

```python
Pattern_0, state_vec_0 = calc_random_pattern_exponential(tau=100, tStart=1000, tDuration=5000, tTotal=7000, channel=0, name='stim exp A')
Pattern_1, state_vec_1 = calc_random_pattern_exponential(tau=100, tStart=1000, tDuration=5000, tTotal=7000, channel=1, name='stim exp B')
Pattern_composed = compose_Patterns([Pattern_0,Pattern_1], 'composition test', total_duration=7000)
Pattern_composed.preview_plot()
```

![ ](https://github.com/grg2rsr/RIOlib/blob/master/screenshot.png  "Example")


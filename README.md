# Keras Attention Mechanism (Hello World)

Simple attention mechanism implemented in Keras. Only 2 lines of code!

```
inputs = Input(shape=(input_dims,))
attention_probs = Dense(input_dims, activation='softmax', name='attention_probs')(inputs)
attention_mul = merge([inputs, attention_probs], output_shape=32, name='attention_mul', mode='mul')
```


Let's consider this Hello World example:

- A vector *v* of 32 values as input to the model (simple feedforward neural network).
- *v[1]* = target.
- Target is binary (either 0 or 1).
- All the other values of the vector *v* (*v[0]* and *v[2:32]*) are purely random and do not contribute to the target.

We expect the attention to be focused on *v[1]* only, or at least strongly. We recap the setup with this drawing:

<p align="center">
  <b>Attention Mechanism explained</b><br><br>
  <img src="assets/attention_1.png" width="400">
</p>

Let's train this model and visualize the attention vector applied to the inputs:

<p align="center">
  <b>Attention Mechanism explained</b><br><br>
  <img src="assets/1.png" width="500">
</p>

We can clearly see that the network figures this out for the inference.

## Behind the scenes

The attention mechanism can be implemented in three lines with Keras:
```
inputs = Input(shape=(input_dims,))
attention_probs = Dense(input_dims, activation='softmax', name='attention_probs')(inputs)
attention_mul = merge([inputs, attention_probs], output_shape=32, name='attention_mul', mode='mul')
```

We apply a `Dense - Softmax` layer with the same number of output parameters than the `Input` layer. The attention matrix has a shape of `input_dims x 32` here.

Then we merge the `Inputs` layer with the attention layer by multiplying element-wise.

Finally, the activation vector (probability distribution) can be derived with:

```
attention_vector = get_activations(m, testing_inputs_1, print_shape_only=True)[1].flatten()
```

Where `1` is the index of definition of the attention layer in the model definition (`Inputs` is indexed by `0`).
layout: true
class: middle, inverse

---

# Modular code development

## Richard Darst from original material by [Radovan Bast](http://bast.fr)

### [NeIC](https://neic.nordforsk.org)/ [UiT The Arctic University of Norway](https://uit.no)

Text is free to share and remix under [CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Code examples: [MIT license](http://opensource.org/licenses/mit-license.html)

Credits: [Jonas Juselius](https://github.com/juselius),
[Roberto Di Remigio](http://totaltrash.xyz),
[Ole Martin Bjørndalen](https://github.com/olemb)

---

## Modularity...

Do you want to use your stuff twice or more?

Or does reinventing the wheel every time sound fun?

Modularity is a primary concept for reusable -and understandable-
code.

---

Modularity is not automatic.  There is a cost, and a payoff:

<img src="img/development-speed.svg" style="width: 80%;"/>

**Discuss:** do you agree or not, and what "properly" means.

.cite[Adapted from ["Simple Made Easy" by Rich Hickey](https://www.infoq.com/presentations/Simple-Made-Easy)]

---

## [The tar pit](https://github.com/papers-we-love/papers-we-love/blob/master/design/out-of-the-tar-pit.pdf)

- Over time software tends to become harder and harder to reason about
- Small changes become harder to implement
- .emph[Bugs start appearing in unexpected places]
- More time is spent debugging than developing
- Complexity strangles development because it does not scale well

.cite[Slide adapted from [Complexity in software development by Jonas Juselius](https://github.com/scisoft/complexity)]


---

## Types of software modularity

- Functions
- Abstraction layers
- Libraries(packages)

---

## Functions: pure is better

A **pure** function has output only a function of input, and no
side-effects.  Same input, *always* same output.

### a) pure: no side effects

```python
def fahrenheit_to_celsius(temp_f):
    temp_c = (temp_f - 32.0) * (5.0/9.0)
    return temp_c

temp_c = fahrenheit_to_celsius(temp_f=100.0)
print(temp_c)
```

### b) stateful: side effects

```python
f_to_c_offset = 32.0
f_to_c_factor = 0.555555555
temp_c = 0.0

def fahrenheit_to_celsius_bad(temp_f):
    global temp_c
    temp_c = (temp_f - f_to_c_offset) * f_to_c_factor

fahrenheit_to_celsius_bad(temp_f=100.0)
print(temp_c)
```

---

## Pure functions are always better


Easier to
- Test
- Understand
- Reuse
- Parallelize
- Simplify
- Optimize
- Compose

But not everything can be pure: Reading data is not pure.  Writing
results is not pure.  But usually the hard part - the science - is
pure.

---

## Recommendations on functions

Try to separate pure from impure parts, and keep your functions small
and to a point.

- I/O is impure
- Keep I/O on the outside and connected
- Keep the inside of your code pure/stateless
- Anything that gets too large, split into functions

<img src="img/good-vs-bad.svg" style="width: 100%;"/>

---

## Abstraction layers

How do you decide what functions to make?  How do functions work
together?

These aren't easy questions.

But almost all serious software is designed in layers.  Typical
layers:

- input
- preprocessing
- transformation
- analysis
- output

In general, put similar code in a nearby location (functions close by,
modules, sub-packages, etc).

Designing software well is an *art*.

---

## Packaging code

You shouldn't have all your code in one palace.

First, divide your stuff into directories with different roles.
Some may be stable and professional, some may be your daily hacks.

Stable code directories depend on others in a clear manner:
e.g. `import other_code`.

Now how do we do that?

---

## Packaging tools

Plenty of tools exist for packaging.  Python has a fairly simple way.

* Make a `setup.py` file
* Optionally, publish it.
* Install it, either from PyPI or from your own checkout:
  - `pip install name`
  - `pip install git+https://github.com/rkdarst/some-package.git#egg=some-package`
  - from git dir, `pip install -e`.  Editing the code immediately
    takes effect, no need to reinstall (good for development)

---

## Example `setup.py`

```python
import setuptools

with open("README.md", "r") as fh:
    long_description = fh.read()

setuptools.setup(
    name="example-pkg-your-username",
    version="0.0.1",
    author="Example Author",
    author_email="author@example.com",
    description="A small example package",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/pypa/sampleproject",
    packages=setuptools.find_packages(),
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
)
```

From reference and tutorial: https://packaging.python.org/tutorials/packaging-projects/


---

## Making packaged code available:

* `pip install`
* `pip install -e` for editable version (changes take effect immediately)
* setting `PYTHONPATH`

---


layout: false

## Questions

- What best practices can you recommend to arrive at well structured, modular
  code in your favourite programming language?
- What would you recommend your colleague who starts in the same programming language?
- How do you deal with code complexity in your projects?

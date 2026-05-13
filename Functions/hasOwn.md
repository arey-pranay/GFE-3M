`Object.hasOwn(objArg, key)` is a **safety check** to ensure the property belongs directly to the object itself, and is not inherited from its prototype chain.

In your current examples/tests, removing it changes nothing because your objects are probably simple plain objects like:

```ts id="x1a2bc"
{
  active: true,
  hidden: false
}
```

These objects don't have enumerable inherited properties, so both versions behave the same.

---

# Why it matters

`for...in` iterates over:

* ✅ Own enumerable properties
* ✅ Inherited enumerable properties

Example:

```ts id="m91zaq"
const parent = {
  inheritedClass: true,
};

const child = Object.create(parent);
child.active = true;

for (const key in child) {
  console.log(key);
}
```

### Output

```ts id="n82psa"
active
inheritedClass
```

Without `hasOwn`, your function would incorrectly include:

```ts id="o22lal"
"active inheritedClass"
```

---

# With `Object.hasOwn`

```ts id="p88sma"
for (const key in child) {
  if (Object.hasOwn(child, key)) {
    console.log(key);
  }
}
```

### Output

```ts id="q11msa"
active
```

Now only direct properties are included.

---

# Why interviews like this

This shows understanding of:

* JavaScript prototype chain
* Property enumeration
* Safer object iteration
* Edge-case defensive coding

Even though most real-world objects are plain objects, interviewers often care about whether you know *why* the safeguard exists.

---

# ⚡ Quick Revision

> `for...in` iterates inherited enumerable properties too, not just own properties.
> `Object.hasOwn()` prevents accidentally including prototype-chain keys.

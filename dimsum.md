---
options:
  end_slide_shorthand: true
theme:
  name: light
  override:
    slide_title:
      colors:
        foreground: "001f3f"
---

<!-- jump_to_middle -->
DimSum
===

<!-- reset_layout -->
![image:width:60%](dimsum.jpg)

---
<!-- font_size: 2 -->

<!-- jump_to_middle -->
# What is DimSum?
<!-- pause -->
A Rocq-based framework for reasoning about multi-language programs.

---
<!-- font_size: 2 -->

<!-- jump_to_middle -->
# A Brief History of Compiler Correctness

---
<!-- font_size: 2 -->
Compiler Correctness Quiz Show
==

<!-- pause -->
## Q: What makes a compiler correct? (S=source)
<!-- pause -->
1. _S and Comp(S) are observationally equivalent_
<!-- pause -->
2. _if S has well-defined semantics, then S and Comp(S) are observationally equivalent_
<!-- pause -->
3. _if S has well-defined semantics and satisfies Spec, then Comp(C) satisfies Spec_
<!-- pause -->
4. _if S is type- and memory-safe, then so is Comp(S)_
<!-- pause -->
5. _Comp(S) is type- and memory-safe_

<!-- pause -->
![image:width:70%](lifeline.jpg)

---
<!-- font_size: 2 -->

Calling...
==
<!-- pause -->
![image:width:30%](leroy.png)

<!-- pause -->
## Solution
 2. _if S has well-defined semantics, then S and Comp(S) are observationally equivalent_

---
<!-- font_size: 2 -->

CompCert: Guarantees
===

<!-- pause -->
![image:width:50%](ccert.jpg)

<!-- pause -->
> "file.asm behaves like file.c"

<!-- pause -->
## Formally
```typst +render
  $forall s forall B in.not "Wrong ." s arrow.b.double B => "Comp(s)" arrow.b.double B$
```
<!-- pause -->
![image:width:90%](semantics.png)

---
<!-- font_size: 2 -->

Compcert: Linking
===
![image:width:45%](printf.jpg)

<!-- pause -->
![image:width:45%](libc.jpg)

<!-- pause -->
> "if S has well-defined semantics [...]"

---
<!-- font_size: 2 -->

Compositional Compiler Correctness
===

<!-- pause -->
## Problem: How do you assign a 'reference' semantics to a partial program?

<!-- pause -->
![image:width:90%](semantics.png)
 
<!-- pause -->
![image:width:80%](spectrum.png)


---
<!-- font_size: 2 -->

Multi-Language Semantics
===

<!-- pause -->
![image:width:30%](multilangprog.jpg)

<!-- pause -->
## Formal Definition of P

<!-- pause -->
```typst +render
  $P = C_1 (S_1) union C_2 (S_2) union ... union C_n (S_n)$
```

<!-- pause -->
- What property do we want to show for P?

<!-- pause -->
## Semantic Refinement

<!-- pause -->
```typst +render
  #let sem(x) = ("⟦" + x + "⟧")
  $sem(P) = sem(C_1 (S_1) union C_2 (S_2) union ... union C_n (S_n))$
```

<!-- pause -->
```typst +render
  #let sem(x) = ("⟦" + x + "⟧")
  $prec.eq sem(S_1) xor sem(S_2) xor ... xor sem(S_n)$
```

---
<!-- font_size: 2 -->

## Semantic Refinement

```typst +render
  #let sem(x) = ("⟦" + x + "⟧")
  $sem(P) = sem(C_1 (S_1) union C_2 (S_2) union ... union C_n (S_n))$
```

```typst +render
  #let sem(x) = ("⟦" + x + "⟧")
  $prec.eq sem(S_1) xor sem(S_2) xor ... xor sem(S_n)$
```

<!-- column_layout: [1,1] -->

<!-- column: 0 -->
<!-- pause -->
### Requires:
  <!-- pause -->
  - definition of semantics
  - definition of refinement
  - composition of semantics

<!-- column: 1 -->
<!-- pause -->
### Goals:
  <!-- pause -->
  - ability to add languages
  - no assumptions about languages

<!-- reset_layout -->
<!-- pause -->
## Beyond Compiler Correctness
```typst +render
  #let sem(x) = ("⟦" + x + "⟧")
  $sem(P) &= sem(C_1 (S_1) union C_2 (S_2) union ... union C_n (S_n))\
  &prec.eq sem(S_1) xor sem(S_2) xor ... xor sem(S_n)$
```
<!-- pause -->
> "Compiled program adheres to the code you wrote."
<!-- pause -->
```typst +render
  #let sem(x) = ("⟦" + x + "⟧")
  $&prec.eq sem("Spec")$
```
<!-- pause -->
> "Compiled program adheres to semantics of some specification."

---

<!-- jump_to_middle -->
Enter DimSum
===

---
<!-- font_size: 2 -->

Does DimSum Adhere?
===

<!-- column_layout: [1,1] -->
<!-- column: 0 -->
<!-- pause -->
### Requires:
<!-- pause -->
  - <span style="color: #4f7942">definition of semantics</span>
  <!-- pause -->
  - <span style="color: #4f7942">definition of refinement</span>
  <!-- pause -->
  - <span style="color: #4f7942">composition of semantics</span>

<!-- column: 1 -->
<!-- pause -->
### Goals:
  <!-- pause -->
  - <span style="color: #4f7942">ability to add languages</span>
  <!-- pause -->
  - <span style="color: #8c3b3b">no assumptions about languages</span>

<!-- reset_layout -->
<!-- column_layout: [1,1] -->
<!-- column: 0 -->
<!-- pause -->
### Allows for:
  <!-- pause -->
  - different memory models
  - different value structures
  - different control flow structure

<!-- pause -->
<!-- column: 1 -->
### But:
  <!-- pause -->
  - no concurrency
  - no typed languages
 
---
<!-- font_size: 2 -->

Semantics in DimSum: Modules and Events
===

- what is a module
- parametric in event lang

---
<!-- font_size: 2 -->

Case Study of DimSum: Rec, Asm, Spec
===

- screenshots of operational semantics

---
<!-- font_size: 2 -->

Example Code
===

- code
- how does a module look like here?

---
<!-- font_size: 2 -->

Proof Outline
===

- show first, second step

---
<!-- font_size: 2 -->

Semantic Linking
===

- show how it is defined as synchronizing on events

---
<!-- font_size: 2 -->

Refinement
===

- simulation of one module of another

---
<!-- font_size: 2 -->

Wrappers
===

- required to reason about semantic differences between languages

---

** Decentralized??
#+ATTR_REVEAL: :frag (appear)
- No fixed source language as spec language
  - Want to be able to link target code that is not representable in
     the source code
  - Important when some things have to be written
     in language B because it would not be possible to write code in
     language A that would do the same thing
- No fixed set of languages
  - Allow language-local reasoning
  - Otherwise proofs about libraries in any one language must take
     into account all the other languages used together in the
     program
    - These proof might not work if new language are added in the
       future
- No fixed memory model
  - asm and high-level languages view memory very differently
- No fixed notion of linking
  - No fixed "official" notion of semantic linking
  - Users can develop new, library-specific notions of linking that
     support higher-level reasoning principles

** Supported languages of Case Study
#+ATTR_REVEAL: :frag (appear)
- Rec: high-level language with recursive functions
- Asm: Assembly-like language
- Spec: language for writing specifications
   
* Key Ideas
- translate programs of all languages into state transition systems (*Modules*)
  - Think of modules as communicating processes
  - Each language \(\mathtt{L}\) has associated set of events
    \(E_{\mathtt{L}}\),
  - Give semantics to a library \(L\) as a module \( [ L
    ]_{\mathtt{L}} \in \text{Module}(E_{\mathtt{L}}) \)
  - Model interactions of modules (e.g. Asm jumps) as synchronization
     on events (e.g. outgoing jumps are synchronized with incoming jumps)
- Library of language-agnostic combinators for linking and
   translating modules
- When we say "a program refines a specification" in formal methods, we mean:
#+begin_quote
The program behaves in a way that is allowed by (or consistent with)
the specification — possibly being more detailed or deterministic,
but never violating the rules or constraints laid out in the
specification.
#+end_quote
- Refinement is defined as simulation on modules

- \(A_{1} \cup_{a} A_{2}\): syntactic linking in Asm
  - Takes two Asm-libraries \(A_{1}\) and \(A_{2}\) and combines their
    program code
- \(\downarrow R\): compilation from Rec-library \(R\) to Asm-library


- Module semantics \([ R ]_{-}\) map  syntactic libraries into
  semantic modules
  - Based on operational semantics of the language
  - Defined precisely in section 4
     

** Modules
- How DimSum assigns meaning to every program component
- Module \(M \in \text{Module}(E)\): a labeled transition system emitting events
  - Events vary from language to language

** Events
- Formalize how modules interact with their environment
- Model interaction of program components as event-based
   communication (synchronization on events)
- Events carry detailed description of the program state

** Example program
\begin{align*}
\mathtt{fn} \ \mathtt{main} () := \
&\mathtt{local} \ x[3]; x[0] \gets 1; x[1] \gets 2; & // x \mapsto [1,2,0] \\ 
&\mathtt{memmove}(x + 1, x + 0, 2); & // x \mapsto [1,1,2] \\
&\mathtt{print}(x[1]); \mathtt{print}(x[2])
\end{align*}


\begin{align*}
\mathtt{fn} \ \mathtt{memmove}(d,s,n) := \
&\mathtt{if} \ \mathtt{locle}(d,s) \\
&\mathtt{then} \ \mathtt{memcpy}(d,s,n,1) \\
&\mathtt{else} \ \mathtt{memcpy}(d+n-1, s+n-1, n -1)
\end{align*}

\begin{align*}
\mathtt{memcpy}(d,s,n,o) := \ &\mathtt{if} \ 0 < n \ \\
&\mathtt{then} \ d \gets !s; \mathtt{memcpy}(d+0, s+0, n-1, 0)
\end{align*}

- \(\mathtt{locle}\) and \(\mathtt{print}\) cannot be implemented in
  Rec

*** Formalization
- Spec: program prints 1 and then 2

- \(\textbf{onetwo} := \ \downarrow \text{main} \ \cup_{a} \downarrow \text{memmove} \cup_{a} \mathbf{locle} \cup_{a} \mathbf{print}\)
- \(\downarrow R: \) Compile Rec-library to Asm-library
  - Compiler explained later

- \(\mathbf{onetwo} \ _{a}\preceq_{s} \mathbf{onetwo}_{\text{spec}}\)

** Semantic linking
- \(M_{1} \ ^{d_{1}}\oplus_{a}^{d_{2}} M_{2}\) talkes two Asm-modules \( M_{1}\) and
  \(M_{2}\) with associated instruction addresses \(d_{1}, d_{2}\) and
  synchronizes them via their jump events
- Takes two Asm transition systems and combines the into a large
   transition system
- Horizontal compositionality
   - Compatibility with refinement
   - Looks similar to par-rule from CSL?

** Semantic wrapper
- Semantic wrapper \(\lceil \cdot \rceil_{r \rightleftharpoons a}\) is an embedding of Rec modules
  into Asm
  - Translates between Rec-events and Asm-events
  - Operate on modules instead of syntactic constructs (in contrast
     to compiler)
- Two important properties
  - \(\downarrow R\) behaves like \(\lceil [ R ] \rceil_{r \rightleftharpoons a}\)
  - Compatible with refinement
** Proof Outline
* Formalization
** Refinement Formally
- this is super technical, we'll need more time here
** Verified Compiler
- we haven't gotten that far
* Discussion
** Strengths
#+ATTR_REVEAL: :frag (appear)
- modules as reasoning domain good idea
** Weaknesses
#+ATTR_REVEAL: :frag (appear)
- No concurrency
- No types
- No liveness properties
- Only toy languages
- Has to verify compiler from lang A to B

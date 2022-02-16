---
title: Boolean Functions 
summary: Here you can find all the codes I created and use in Sage for my research on Boolean functions.
date: '2021-01-24'
type: book
math: true

---

***
## Introduction 
***

This page was created to exchange functions and computations obtained in $\texttt{Sage}$ for the research regarding Boolean functions.

The following lines need to be added to your $\texttt{Sage}$ notebook to support cryptographical functions that are available (for more info see [Sage-BooleanFunctions](https://doc.sagemath.org/html/en/reference/cryptography/sage/crypto/boolean\_function.html) and [Sage-SBox](https://doc.sagemath.org/html/en/reference/cryptography/sage/crypto/sbox.html)).




```python
from sage.crypto.sbox import SBox
from sage.crypto.boolean_function import BooleanFunction
```

***
## Basic tools
***
Below we give some basic tools which will be used going onwards, including: 
- Walsh support of a Boolean function $f\in\mathcal{B}\_n$
- The inverse WHT given a Walsh spectra $w$ of length $2^n$
- Checking wheather a Boolean function $f$ in odd dimension is semi-bent
- Walsh spectra of a vectorial Boolean function $F$
- Checking whether an $(n,m)$-function $F$ is AB, if $F$ is defined by a list of its coordinates (If $F$ is given in integer form, there is already an integrated function in Sage)

```python
def walsh_support(f,n):
    w=BooleanFunction(f).walsh_hadamard_transform();
    l=len(w);
    V=VectorSpace(GF(2),n);
    ws=[];
    for v in V:
        i=ZZ(list(v),base=2);
        if(w[i] !=0):
            ws.append(vector(GF(2),v));
    return ws;

def inverseWHT(w,n):
    t=[];
    V=sorted(VectorSpace(GF(2),n));
    for u in V:
        s=0;
        for x in V:
            i=ZZ(list(x), base=2);
            s=s+w[i]*(-1)^(vector(u,GF(2))*vector(x,GF(2)));
        t.append((1-(2^(-n))*s)/2);
    return t;


def is_semibent(f,n):
    if((f.is_plateaued()) and (2^((n+1)/2) in f.walsh_hadamard_transform())):
        return True;
    else:
        return False;

def walsh_spectrum(F):
    n=len(F);
    S=[];
    for i in [1..2^n-1]:
        f=F.component_function(i);
        S=S+list(f.walsh_hadamard_transform());
    K=sorted(list(set(S)));
    return [[K[i],S.count(K[i])] for i in [0..len(K)-1]];
    
def isAB(M):
    T=list(transpose(matrix(M)));
    T1=[ZZ(list(x),base=2) for x in T];
    T1=SBox(T1);
    return T1.is_almost_bent();

```

***
## $\mathcal{C}$ and $\mathcal{D}$ classes 
***

The Maiorana-McFarland class $\mathcal{M}$ is the set of $m$-variable ($m=2n$) Boolean
functions of the form

$$f(x,y)=x \cdot \pi(y)+ g(y), ox{ for all } x, y\in\mathbb{F}\_2^n,$$

where $\pi$ is a permutation on $\mathbb{F}\_2^n$, and $g$ is an arbitrary Boolean function on
$\mathbb{F}\_2^n$. From this class Carlet derived the  $\mathcal{C}$ class of bent functions that contains all functions of the form

$$f(x,y) = x\cdot\pi(y)+ \mathbf{1}\_{L^{\perp}}(x)$$

where $L$ is any linear subspace of $\mathbb{F}\_2^n$, $\mathbf{1}\_{L^{\perp}}$ is the indicator function of the space $L^{\perp}$, and
$\pi$ is any permutation on $\mathbb{F}\_2^n$ such that:


$(C)$ $\phi(a + L)$ is a flat (affine subspace),  for all $a\in \mathbb{F}\_2^n$, where $\phi:=\pi^{-1}$.

The permutation $\phi$ and the subspace $L$  are then said to satisfy the $(C)$ property, or for short \_$(\phi,L)$ has property $(C)$\_.

Another class introduced by Carlet, called $\mathcal{D}$, is defined similarly as
$$f(x, y)=x\cdot\pi(y) + \mathbf{1}\_{E\_1}(x) \mathbf{1}\_{E\_2}(y)$$

where $\pi$ is a permutation on $\mathbb{F}\_2^{n}$ and $E\_1,E\_2$ two linear subspaces of $\mathbb{F}\_2^n$ such that $\pi(E\_2)=E\_1^{\perp}$.

We will define three $\texttt{Sage}$ functions: 
- the indicator function $\mathbf{1}\_A(x)$ which will be denoted with $\texttt{ind(x,A)}$, 
- the trace function $Tr\_m^n(x)$ denoted with $\texttt{tr(x,m,n)}$ and 
- the anihilator of a linear space $E$ of $\mathbb{F}\_{2^m}$ denoted with $\texttt{dualSpace(E,m)}$.


```python
#Fm=GF(2^m);
def ind(x,L):
    if x in L:
        return Fm.one();
    else:
        return Fm.zero();


def tr(x,m,n):
    return sum([x^(2^(i*m)) for i in [0..(n/m)-1]])


def dualSpace(E,m):
    Fm=GF(2^m);
    t=[];
    for x in Fm:
        b=0;
        for y in E:
            if (x*y).trace()==0:
                b=b+1;
        if b==len(E):
            t.append(x)
    return t;
```

**Example:** Let $m=9$ and $s=3$. Then $m/s$ is odd. Furthermore, for $d=284$ we have that $d(2^s+1) \mod (2^9-1)=1$. Let $U=\\{1,\alpha,\alpha^2\\}$ where $\alpha$ is a primitive element of $\mathbb{F}\_{2^3}$ such that $\alpha^3+\alpha+1=0$. 

For $L=\langle U\rangle$ we have that $(\pi^{-1},L)$ satisfies the $(C)$ property. Hence, the function $f:\mathbb{F}\_{2^m}\times\mathbb{F}\_{2^m}\to\mathbb{F}\_2$ defined with $$f(x,y) = x\cdot\pi(y)+ \mathbf{1}\_{L^{\perp}}(x)$$ is bent.


```python
Fm=GF(2^9);
sFm=sorted(Fm);

p=Fm.primitive_element();
a=p^((2^9-1)/(2^3-1));
u=sFm[13];

Lperp=[x for x in Fm if ((x).trace()+1)*((x*a).trace()+1)==1];

f=[(x*y^284).trace()+ind(x,Lperp) for x in sFm for y in sFm];
f=BooleanFunction(f);
f.is_bent()
```

**Example:** Let $m=9$ and $s=3$. Then $m/s$ is odd. Furthermore, for $d=284$ we have that $d(2^s+1) \mod (2^9-1)=1$. Let $\alpha$ be a primitive element of $\mathbb{F}\_{2^3}$ such that $\alpha^3+\alpha+1=0$. 

For $E\_2=\langle \alpha,\alpha^2 \rangle$ we have that $\pi(E\_2)=E\_2^{\perp}=E\_1$. Hence, the function $f:\mathbb{F}\_{2^m}\times\mathbb{F}\_{2^m}\to\mathbb{F}\_2$ defined with $$f(x,y) = x\cdot\pi(y)+ \mathbf{1}\_{E\_1}(x)\mathbf{1}\_{E\_2}(y)$$ is bent.


```python
Fm=GF(2^9);
sFm=sorted(Fm);

p=Fm.primitive_element();
a=p^((2^9-1)/(2^3-1));

E2=[Fm.zero(),a^2,a+a^2,a];
E1=dualSpace(E2,9);

f=[(x*y^284).trace()+ind(x,E1)*ind(y,E2) for x in sFm for y in sFm];
f=BooleanFunction(f);
f.is_bent()
```

Let $\pi$ be a permutation on $\mathbb{F}\_{2^m}$, $L\subset\mathbb{F}\_{2^m}$ be a linear subspace of $\mathbb{F}\_{2^m}$ such that $(\pi^{-1},L)$ satisfies the $(C)$ property, and let $E\_1,E\_2\neq \\{0\\}$ be two linear subspaces of $\mathbb{F}\_{2^m}$ such that $\pi(E\_2)=E\_1^{\perp}$. Then the class of bent  functions $f:\mathbb{F}\_{2^m}\times\mathbb{F}\_{2^m}\to\mathbb{F}\_{2}$ containing all functions of the form  
$$f(x,y)=Tr\_1^m(x\pi(y))+a\_0\mathbf{1}\_{L^{\perp}}(x)+a\_1\mathbf{1}\_{E\_1}(x)\mathbf{1}\_{E\_2}(y), \,\;  a\_i \in \mathbb{F}\_{2}$$
is called $\mathcal{CD}$ and is a superclass of $\mathcal{C}$ and $\mathcal{D}$.

**Example:** Let $m=9$ and $s=3$. Then $m/s$ is odd. Furthermore, for $d=284$ we have that $d(2^s+1) \mod (2^9-1)=1$.  Let $U=\\{1,\alpha,\alpha^2\\}$ where $\alpha$ is a primitive element of $\mathbb{F}\_{2^3}$ such that $\alpha^3+\alpha+1=0$. 

For $L=\langle U\rangle$ we have that $(\pi^{-1},L)$ satisfies the $(C)$ property. For $E\_2=\langle \alpha,\alpha^2 \rangle$ we have that $\pi(E\_2)=E\_2=E\_1^{\perp}$. Hence, the function $f:\mathbb{F}\_{2^m}\times\mathbb{F}\_{2^m}\to\mathbb{F}\_2$ defined with $$f(x,y) = x\cdot\pi(y)+ \mathbf{1}\_{L^{\perp}}(x)+\mathbf{1}\_{E\_1}(x)\mathbf{1}\_{E\_2}(y)$$ is bent.


Below we give a program (depending if we are using vector space or finite field notation), which checks if $(\pi^{-1},L)$ satisfies the $(C)$ property. The permutations $\texttt{pi_inv}$ are given in their integer representation.

```python
def shift_space(W,u):
    new=[];
    for v in W:
        new.append(vector(GF(2),v)+vector(GF(2),u));
    return new;

def is_flat(A,n):
    V=VectorSpace(GF(2),n);
    A1=V.subspace(A);
    if len(list(A1))==len(A):
        return True;
    else:
        return False;

def is_closed(F):
    C=Combinations([0..len(F)-1],2);
    C=C.list();
    G=Graph();
    for i in C:
        if F[i[0]]+F[i[1]] in F:
            G.add_edge([i[0],i[1]])
    if(len(G.clique_maximum())==len(F)):
        return True;
    else:
        return False;

def satisfiesC(pi_inv,L,n):
    t=[];
    Fn=GF(2^n);
    Vn=Fn.vector_space();
    for a in Vn:
        A=shift_space(L,a);
        F=[Vn(pi_inv[ZZ(list(x),base=2)]) for x in A];
        t.append[is_flat(F,n)];
    return t;

def satisfiesC_field(pi_inv,L,n,F):
    Fk=F;
    for a in Fk:
        if a!=Fk.zero():
            A=[pi_inv(a+L[i]) for i in [0..3]];
            B=[A[0]+A[i] for i in [0..3]];
            if (0 in B) and (is_closed(B)):
                return True;
            else:
                return False;
```

## Inclusion/exclusion in $\mathcal{M}^{\\#}$

A bent function $f$ in $n$ variables belongs to $\mathcal{M}^{\\#}$ if and only if there exists an $\frac{n}{2}$-dimensional linear subspace $V$ of $\mathbb{F}\_2^n$ such that the second-order derivatives $$ D\_{a}D\_{b}f(x)=f(x)\oplus f(x\oplus a)\oplus f(x\oplus b)\oplus f(x\oplus a\oplus b)$$ vanish for any $ a,b \in V$.

```python
def is_in_MM(f,n):
    s=[];
    for a in [1..2^n-1]:
        for b in [a+1..2^n-1]:
            if set(ttab(f.derivative(a).derivative(b)))=={0}:
                s.append([a,b]);
    G=Graph();
    G.add_edges(s);
    cl=list(sage.graphs.cliquer.all_cliques(G,2^(n/2)-1,2^(n/2)-1));
    V=VectorSpace(GF(2),n);
    V1=sorted(V);
    b1=[V.subspace([V1[0]]+[V1[i] for i in s]) for s in cl];
    for K in b1:
        if len(K)==2^(n/2):
            return True;
    return False;
```

**Example:** The function $f\in \mathcal{D}\_0$ defined by $f(x,y)=Tr\_1^4(xy^7)+\delta\_0(x)$ for $x,y\in\mathbb{F}\_{2^4}$ is a bent function outside $\mathcal{M}^{\\#}$.

```python
Fm=GF(2^4);
sFm=sorted(Fm);

f=[(x*y^7).trace()+ind(x,[0]) for x in sFm for y in sFm];
f=BooleanFunction(f);
is_in_MM(f,8)
False
```
## Duals of Boolean function

Depending on wheather $f$ is a bent or plateaued function, we have different notions of duals. 

For a bent Boolean function $f$ defined on $\mathbb{F}\_2^n$, its dual $f^{\star}$ is defined as a function from $\mathbb{F}\_2^n$ to $\mathbb{F}\_2$, for which it holds that $$(-1)^{f^{\star}( {x})}=2^{-\frac{n}{2}}W_f( {x}),\ \  {x}\in \mathbb{F}\_2^n.$$ A standard way of  defining the dual $\tilde{f}:\mathbb{F}\_2^n\to\mathbb{F}\_2$ of an $s$-plateaued  Boolean function $f$ on $\mathbb{F}\_2^n$ is as follows: $$\tilde{f}(x)=2^{-\frac{n+s}{2}}|W\_f( {x})|,\ \  {x}\in\mathbb{F}\_2^n.$$

```python
def bent_dual(f,n): # f is bent
    t=[];
    w=f.walsh_hadamard_transform();
    for x in w:
        if x==2^(n/2):
            t.append(0);
        else:
            t.append(1);
    return BooleanFunction(t);

def stand_dual(f,n): # f is s-plateaued
    t=[];
    w=BooleanFunction(f).walsh_hadamard_transform();
    for x in w:
        if x==0:
            t.append(0);
        else:
            t.append(1);
    return BooleanFunction(t);
```
Let  $S_f=v\oplus E=\\{\omega\_0,\ldots,\omega\_{2^{k-s}-1}\\} \subset \mathbb{F}\_2^k$, for some $v \in \mathbb{F}\_2^k$ and linear subspace $E=\\{e\_0,e\_1, \ldots, e\_{2^{k-s}-1}\\}\subset \mathbb{F}^k\_2$. For a function $f^{\star}:\mathbb{F}\_2^{k-s}\rightarrow \mathbb{F}\_2$ with $wt(f^{\star})=2^{k-s-1}\pm 2^{\frac{k-s}{2}-1}$, let the Walsh spectrum of $f$ be defined (by identifying $x\_i \in \mathbb{F}\_2^{k-s}$ and $e\_i \in E$) as
$W\_f(u)= 2^{\frac{k+s}{2}}(-1)^{f^{\star}(x\_i)}$ for $u= v\oplus e\_i \in S\_f$, and $W\_f(u)=0$ otherwise. Then $f$ is an $s$-plateaued function if and only if $f^{\star}$  is a bent function on $\mathbb{F}^{k-s}\_2$.

```python
def plateFromBent(S,ttD,s,n):
    ws=[0 for i in [0..(2^n)-1]];
    j=0;
    for x in S:
        i=ZZ(list(x), base=2);
        ws[i]=2^((n+s)/2)*((-1)^(int(ttD[j])));
        j=j+1;
    return inverseWHT(ws);
```
## $(P\_U)$ property

We say that a Boolean function $g\in\mathcal{B}\_n$ satisfies the property $(P\_U)$ with the defining set $U\subseteq \mathbb{F}\_2^n$ if $D\_uD\_vg\equiv 0$ for any $u,v\in U$. The program below finds the largest set $U$ for which this property is satisfied.

```python
def Property_PTau(g,n):
    FF=sorted(GF(2^n));
    l=[sorted([x,y]) for x in [1..2^n-1] for y in [x+1..2^n-1] if g.derivative(x).derivative(y).algebraic_degree()==-1];
    G=Graph();
    G.add_edges(l);
    return G.cliques_maximum();
```
## Functions $\varphi_u$ and $\psi_u$

For the definition and application of the functions $\varphi_u$ and $\psi_u$ we reffer to the following article of BPH - [CLICK HERE.](https://eprint.iacr.org/2021/550)

```python
def phi(u,fd,gd,Sf,Sg):
    L=[s for s in Sf if s not in intersection(Sf,Sg)];
    tt=[];
    for x in L:
        i=Sf.index(x);
        a=fd[i];
        v=vector(GF(2),u)+vector(GF(2),x);
        j=Sg.index(v);
        b=gd[j];
        tt.append((a+b)%2);
    return tt;

def psi(u,fd,gd,Sf,Sg):
    L1=intersection(Sf,Sg);
    tt=[];
    for x in L1:
        i=Sf.index(x);
        a=fd[i];
        v=vector(GF(2),u)+vector(GF(2),x);
        j=Sg.index(v);
        b=gd[j];
        tt.append((a+b)%2);
    return tt;
```

## Constructions of 4-bent decompositions outside $\mathcal{M}^{\\#}$

Let  $g\notin \mathcal{M}^{\\#}$ be a bent function in $n$ variables. For an arbitrary matrix $M\in GL(n,\mathbb{F}\_2)$ and vector ${c}\in \mathbb{F}\_2^n$, let $S\_f=({c}\oplus \mathbb{F}\_2^nM)\wr T\_{t\_1}\wr T\_{t\_2}$, where $t\_1,t\_2\in\mathcal{B}\_{n}$ such that $g({x},{y})\oplus v\_1t\_1({x},{y})\oplus v\_2t\_2({x},{y})$ is bent for any $ v\_1,v\_2\in\mathbb{F}\_2$, where $ {x},{y}\in\mathbb{F}\_2^{n/2}$. Let $Q=\\{\mathbf{0}\_n\\}\times \mathbb{F}\_2^{2}=\\{{q}\_0,{q}\_1,{q}\_2,{q}\_3\\}$ and set $S\_{f\_i}={q}\_i \oplus S\_f$, for $i=0,\ldots,3$. Then, the functions $f\_i \in \mathcal{B}\_{n+2}$, constructed using Theorem 3.3 in [HPWZ](https://arxiv.org/abs/1811.04171), with $S\_{f\_i}$ and $g$, are semi-bent functions on $\mathbb{F}\_2^{n+2}$ with pairwise disjoint spectra. Moreover, the function $\mathfrak{f}\in\mathcal{B}\_{n+4}$, whose restrictions are  $\mathfrak{f}|\_{{a}\_i\oplus \mathbb{F}\_2^{n+4}}=f\_i$ (thus $\mathfrak{f}=f\_1||f\_2||f\_3||f\_4$), where ${a}\_i\in\{\mathbf{0}\_{n+2}\}\times \mathbb{F}\_{2}^2$, is a bent function  outside $\mathcal{M}^{\\#}$.

**Example:** Below is an example in dimension $8$ for $t\_1=t\_2=\delta\_0$ and $g\in\mathcal{D}\_0\setminus\mathcal{M}^{\\#}$.

```python
def concat(tt):
    s=[];
    for x in tt:
        s=s+x;
    return BooleanFunction(s)

def delta(x):
    if x==0:
        return 1;
    else:
        return 0;


n=8;
G=GL(n,GF(2));
M=G.random_element();
E=VectorSpace(GF(2),n);
c=E.random_element();
S1=[c+M*x for x in E];
t1=[delta(x) for x in sorted(GF(2^4)) for y in sorted(GF(2^4))];

Sf=[list(S1[i])+[t1[i],t1[i]] for i in [0..255]]
Sf2=[vector(GF(2),x)+vector([0,0,0,0,0,0,0,0,0,1]) for x in Sf]
Sf3=[vector(GF(2),x)+vector([0,0,0,0,0,0,0,0,1,0]) for x in Sf]
Sf4=[vector(GF(2),x)+vector([0,0,0,0,0,0,0,0,1,1]) for x in Sf]
SF=[Sf,Sf2,Sf3,Sf4];

F=[];
g=[(x*y^7).trace()+delta(x) for x in sorted(GF(2^4)) for y in sorted(GF(2^4))]
f1=[(-1)^(g[i]) for i in [0..len(g)-1]];
S1=[[ZZ(list(x),base=2) for x in A] for A in SF]
for s1 in S1:
    t=[0 for i in [0..2^(10)-1]];
    j=0;
    for i in s1:
        t[i]=(2^((n+4)/2)*f1[j]);
        j=j+1;
    F.append(inverseWHT(t,10));

G=concat(F);
G=BooleanFunction(G);
```


2025-05-02T01:21:15-07:00
Been playing with this to show why a laser generates coherent state.
- https://x.com/jwt0625/status/1916675616524415121
However found the wigner function is blowing up.
Spent the whole night, and found the beam splitter U matrix is bullshit:
![[Pasted image 20250502012150.png]]


Even worse with pi/4 instead of pi/15 mixing angle:
![[Pasted image 20250502012344.png]]

So this beam_splitter is doing nonsense:
```python
def beam_splitter(cutoff, theta=np.pi / 15):

"""

Return the 2-mode unitary for a loss-less beam splitter with mixing

angle θ (θ = π/4 → 50:50).

"""

a = destroy(cutoff)

a1 = tensor(a, qeye(cutoff))

a2 = tensor(qeye(cutoff), a)

return (-1j * theta * (a1.dag() * a2 - a1 * a2.dag())).expm()
```

2025-05-02T01:26:47-07:00

there should not be a `-1j`!!!
```python
from qutip import destroy, qeye, tensor

def beam_splitter(cutoff, theta=np.pi/4):
    """
    50:50 BS when theta=pi/4.  Exponent is anti-Hermitian:
       U = exp[  theta * (a1 a2^dag - a1^dag a2)  ].
    """
    a  = destroy(cutoff)
    a1 = tensor(a, qeye(cutoff))
    a2 = tensor(qeye(cutoff), a)
    # (a1 * a2.dag() - a1.dag() * a2) is anti-Hermitian,
    # so multiplying by real theta keeps A† = –A.
    return (theta * (a1 * a2.dag() - a1.dag() * a2)).expm()

```


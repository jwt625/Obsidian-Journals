2025-02-17T20:57:09-08:00
See this issue:
- https://github.com/sharkdp/numbat/issues/481#issuecomment-2661427325
- https://github.com/search?q=repo%3Asharkdp%2Fnumbat%20init.nbt&type=code
- https://numbat.dev/doc/cli-customization.html

`/Users/wentaojiang/Library/Application Support/numbat`

`cmd + shift + .` to show hidden file on macos finder. Stupid shit.

Ugh need to get the newest version:
![[Pasted image 20250217210911.png]]


Following
- https://numbat.dev/doc/cli-installation.html

Installing rust lol:
![[Pasted image 20250217211127.png]]

A lot of shit going on:
![[Pasted image 20250217211535.png]]


2025-02-17T21:42:36-08:00
Hell yeah:
![[Pasted image 20250217214251.png]]

The `init.nbt`:
```
fn magic_energy<D: Dim>(q: D) -> Energy =

if is_zero(q)

then 0

else if is_dimensionless(q / s) # q is a time: E = h / t

then ℎ / quantity_cast(q, s)

else if is_dimensionless(q / K) # q is a temperature: E = k_B T

then k_B quantity_cast(q, K)

else if is_dimensionless(q / m) # q is a wavelength: E = ℎ c / λ

then ℎ c / quantity_cast(q, m)

else if is_dimensionless(q * m) # q is a wavenumber: E = ℎ c * k

then ℎ c * quantity_cast(q, 1/m)

else if is_dimensionless(q / g) # q is a mass: E = m c²

then quantity_cast(q, g) * c²

else if is_dimensionless(q / (cal/mol)) # q is energy per mole: E = q / N_A

then quantity_cast(q, cal/mol) / N_A

else error("magic_energy: Cannot convert quantity of unit '{unit_name(q)}' to an energy")

  

fn for_spectroscopists<D: Dim>(q: D) -> Wavenumber =

if is_dimensionless(q / J) # q is an Energy

then quantity_cast(q, J)/(ℎ c) -> cm^-1

else

magic_energy(q)/(ℎ c) -> cm^-1
```


---
layout: project
type: project
image: images/double_pendulum_pygame.png
title: Multipendel
permalink: projects/double_pendulum_pygame
# All dates must be YYYY-MM-DD format!
date: 2022-02-28
labels:
  - Multipendel
  - Python
  - Runge-Kutta-Methoden
summary: Ich stelle einige Simulationen eines Doppelpendels vor.
---

In Vorbereitung auf meine Doktorarbeit habe ich mich mit Mehrkörpermodellen auseinandergesetzt. 
Dabei habe ich mit Multipendeln, also Ketten von $N \geq 2$ Punktmassen $m$, die durch masselose Stäbe miteinander verbunden sind.

## Doppelpendel $N=2$

### Bewegungsgleichung

<div class="ui small rounded images">
  <img class="ui image" src="../images/double_pendulum_scheme.png">
</div>

Wir betrachten ein Doppelpendel mit Punktmassen $m_1$, die durch einen Stab der Länge $l_1$ mit dem Koordinatenursprung verbunden ist, und $m_2$, die durch einen Stab der Länge $l_2$ an $m_1$ befestigt ist.
Den Zustand eines Doppelpendels für einen Zeitpunkt $t$ kann man eindeutig mittels der Winkel $\alpha_1$ und $\alpha_2$. 
Fasst man diese beiden Winkel in einem Vektor $q:=(\alpha_1,\alpha_2)^T$ zusammen, so kann man die Bewegung des Doppelpendels in Abhängigkeit des Vektors $q$ bzw. dessen Ableitungen nach der Zeit darstellen.
Die Herleitung erfolgt mit den sogenannten *Euler-Lagrange-Gleichungen*
$$\frac{\mathrm{d}}{\mathrm{d}t}\left(\frac{\partial L}{\partial \dot{q}_k}(q,\dot{q})\right)-\frac{\partial L}{\partial q_k}(q), \qquad (k=1,2).$$
Hier bezeichne $$L(q,\dot{q}):=T(q,\dot{q})-U(q)$$
die Lagrange-Funktion, die als Differenz aus kinetischer Energie $T(q,\dot{q})$ und potenzieller Energie $U(q)$ definiert ist.
Die Punkte über den Variablen bezeichnen dabei stets Ableitungen nach der Zeit.
Geht man davon aus, dass $m_1=m_2=:m$ und $l_1=l_2=:l$ ist, so kann man keinetische und potenzielle Energie schreiben als

$$T(q,\dot{q})=\frac{ml^2}{2}\left(2\dot{\alpha}_1^2+2\cos(\alpha_2-\alpha_1)\dot{\alpha}_1\dot{\alpha}_2+\dot{\alpha}_2^2\right)$$
und 
$$U(q)=-mgl(2\cos(\alpha_1)+\cos(\alpha_2)),$$
wobei $g=9.81\frac{m}{s^2}$ die Fallbeschleunigung ist.

Setzt man dies in die Euler-Lagrange-Gleichungen ein, so erhält man die Bewegungsgleichungen

$$\left( \begin{array}{cc}
2 & \cos(\alpha_2-\alpha_1)  \\ 
\cos(\alpha_2-\alpha_1)  & 1  \\
\end{array}\right)
\left( \begin{array}{c}
\ddot{\alpha}_1   \\ 
\ddot{\alpha}_2   \\
\end{array}\right)
=\left( \begin{array}{c}
-2\frac{g}{l}\sin(\alpha_1)+\sin(\alpha_2-\alpha_1)\dot{\alpha}_2^2   \\ 
-\frac{g}{l}\sin(\alpha_2)-\sin(\alpha_2-\alpha_1)\dot{\alpha}_1^2   \\
\end{array}\right).$$

Dies ist ein lineares Differentialgleichungssystem 2. Ordnung in impliziter Darstellung.

Um dieses Differentialgleichungssystem mit einem Runge-Kutta-Verfahren numerisch lösen zu können, muss man es in ein äquivalentes System erster Ordnung transformieren.
Setze dazu $z:=\left( \begin{array}{c}
q   \\ 
\dot{q}  \\
\end{array}\right)
=\left( \begin{array}{c}
\alpha_1 \\
\dot{\alpha_1}  \\ 
\alpha_2   \\
\dot{\alpha_2}
\end{array}\right).$
Dann ist 
$$\left( \begin{array}{cccc}
1&0&0&0\\
0&2&0 & \cos(z_3-z_1)  \\ 
0&0&1&0\\
0&\cos(z_3-z_1) &0 & 1  \\
\end{array}\right)
\dot{z}
=\left( \begin{array}{c}
z_2\\
-2\frac{g}{l}\sin(z_1)+\sin(z_3-z_1)z_4^2   \\ 
z_4\\
-\frac{g}{l}\sin(z_3)-\sin(z_3-z_1)z_2^2   \\
\end{array}\right).$$

ein dazu äquivalentes System erster Ordnung.


### Animationen des Doppelpendels

1.  mittels `Pygame`


![Double pendulum](../images/double-pendulum.gif)






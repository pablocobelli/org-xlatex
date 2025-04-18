
# Table of Contents

1.  [About org-xlatex](#org329934a)
2.  [About this fork](#org21e8336)
3.  [Demo video [&#x2026;]](#org2448904)
4.  [Examples](#org414c235)
    1.  [One-line displayed equations](#orgada7f92)
    2.  [Multiple equations in align environment](#orga869a3a)
    3.  [Matrices](#orga3bca68)
    4.  [Live example](#orgc4eed50)
5.  [Installation](#org971b7ad)
6.  [Configuration](#orgda2023a)
    1.  [[Optional] Custom default (minimal) height for previews](#org7f4b2b8)
    2.  [[Optional] Custom function for better preview positioning](#orgeef95b3)
7.  [Links to the original project and author](#org38cb137)



<a id="org329934a"></a>

# About org-xlatex

This is org-xlatex, a package that provides instant latex preview for org buffers using xwidget and mathjax. This repo is a fork of the code found at

<https://github.com/ksqsf/org-xlatex>,

originally authored by user ksqsf and contributed to by user LeSuisse.


<a id="org21e8336"></a>

# About this fork

As of this writing, this fork introduces an enhancement to the preview windows: they **automatically resize based on the dimensions of the content rendered by MathJax**. This ensures that the previews fit precisely around the output, avoiding overflow or excessive whitespace.

Further (minor) improvements may be added in future updates.


<a id="org2448904"></a>

# Demo video [&#x2026;]


<a id="org414c235"></a>

# Examples

Here are some examples of the type of previews `org-xlatex` can generate, showcasing the new capability added in this fork: **dynamically adjusting preview size to match the dimensions of the rendered content**.


<a id="orgada7f92"></a>

## One-line displayed equations

Complex potential in 2D potential flow theory

$$
W \left( z \right) = \phi + i \psi.
$$

Navier-Stokes equations for newtonian fluid in vector form

$$
\frac{\partial \vec{u} }{\partial t} + \left( \vec{u} \cdot \vec{ \nabla }   \right) \vec{u} = - \frac{ 1 }{ \rho } \vec{ \nabla } p + \nu \nabla^2 \vec{u}.
$$

Levi-Civita and Kronecker Delta

\begin{equation}
\epsilon _{ijk} \epsilon _{ilm} = \delta _{jl} \delta _{km} - \delta _{jm} \delta _{kl}.
\end{equation}


<a id="orga869a3a"></a>

## Multiple equations in align environment

Lorenz equations are a good example

\begin{align}
\frac{d x}{d t} &= \sigma \left( y - x \right) , \\
\frac{d y}{d t} &= x \left( \rho - z \right) - y, \\
\frac{d z}{d t} &= x y - \beta z.
\end{align}

is a well known system of three coupled ordinary differential equations.


<a id="orga3bca68"></a>

## Matrices

Simple 2 by 2 matrix:

$$
A = \left( \begin{array}{cc}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{array} \right).
$$

A 3 by 3 version:

$$
B = \left( \begin{array}{cc}
b_{11} & b_{12} & b_{13} \\
b_{21} & b_{22} & b_{23} \\
b_{31} & b_{32} & b_{33}
\end{array} \right).
$$


<a id="orgc4eed50"></a>

## Live example

A live example for the video.

\begin{equation}
\label{eq:4}
\vec{\nabla} \times \vec{E} = - \frac{\partial \vec{ B }  }{\partial t} .
\end{equation}


<a id="org971b7ad"></a>

# Installation

The package is not in any official online repository for Emacs packages. In order to install it, clone this repo locally and follow the usual procedure for adding packages to your Emacs system.

Doom Emacs users can add the following:

    (package! org-xlatex
      :recipe (:host github
               :repo "pablocobelli/org-xlatex"))

to their configuration file `packages.el`, and run `doom sync` for the changes to take effect.


<a id="orgda2023a"></a>

# Configuration

    (use-package! org-xlatex
      :after (org)
      :hook (org-mode . org-xlatex-mode))


<a id="org7f4b2b8"></a>

## [Optional] Custom default (minimal) height for previews

It is suggested to also customize a value for `org-xlatex-height`, setting it to a rather low value (in pixels). The actual height used in the previews would be determined as the maximum between this value and that determined by `org-xlatex`. An example of such configuration is the following:

    (setq org-xlatex-height 10)

which can be added under the `:config` section in the `use-package!` declaration.


<a id="orgeef95b3"></a>

## [Optional] Custom function for better preview positioning

The original code allows for customizing the position of floating previews. The lines below define a custom function, `org-xlatex-position-function`, which centers the previews horizontally within the frame and places them vertically below the cursor.

    (after! org-xlatex
    (setq org-xlatex-position-function
          (lambda (_xy)
            (let* ((edges (window-inside-pixel-edges)) ; (LEFT TOP RIGHT BOTTOM)
                   (win-left (nth 0 edges))
                   (win-top (nth 1 edges))
                   (win-width (- (nth 2 edges) win-left))
                   (win-height (- (nth 3 edges) win-top))
    
                   ;; size of the floating widget
                   (widget-size (funcall org-xlatex-size-function (cons org-xlatex-width org-xlatex-height)))
                   (widget-width (car widget-size))
                   (widget-height (cdr widget-size))
    
                   ;; cursor position (in pixels, relative to the window)
                   (cursor-pos (posn-at-point))
                   (cursor-y (when cursor-pos
                               (cdr (posn-x-y cursor-pos))))
                   (line-height (frame-char-height))
    
                   ;; position relative to the frame
                   (x (+ win-left (/ (- win-width widget-width) 2)))
                   (y (+ win-top (or cursor-y 0) (* 2 line-height))))
              (cons x y)))))


<a id="org38cb137"></a>

# Links to the original project and author

-   Original project: <https://github.com/ksqsf/org-xlatex>
-   Original author: ksqsf <https://github.com/ksqsf>


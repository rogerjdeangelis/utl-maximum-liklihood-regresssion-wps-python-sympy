# utl-maximum-liklihood-regresssion-wps-python-sympy
https://stackoverflow.com/questions/63274255/using-maximum-likelihood-to-derive-regression-coefficients-in-python
    %let pgm=utl-maximum-liklihood-regresssion-wps-python-sympy;

    Maximum liklehood regresssion wps python sympy

    github
    https://tinyurl.com/334wymun
    https://github.com/rogerjdeangelis/utl-maximum-liklihood-regresssion-wps-python-sympy

    stackoverflow
    https://tinyurl.com/3teddpwd
    https://stackoverflow.com/questions/63274255/using-maximum-likelihood-to-derive-regression-coefficients-in-python

    Solution by
    https://stackoverflow.com/users/8926905/siong-thye-goh

    Odrdinary least squares is a special case of maximum liklihood MLE).
    MlE can work with a wide range of distributions.
    MLE is frequently used in theorectical researcf?

    Note this is a trivial example and the result matchesodinary Least Squares(OLS).
    It does provide a template for solving other functions.

    Minimize this objective function (clasical MLE objective function)
        n
       ---
       \                            2
     -  .   (yi - alpha - beta * xi)
       /
       ---
        1

    Easier to solve using log of liklihood function


    PROBLEM SOLVE y = a + bx for a closed form solution

          -+-----------+-----------+-----------+-----------+-
          |                                                 |
          |  Solve with MLE y = a + b*x                     i
      3.5 +                                   X    Y        |
          |                                                 +
          |                                   2    1        |
      3.0 +            X (2,3)                3    2        |
          |                                   2    3        +
          |                                                 |
      2.5 +                                                 +
          |  OLS Regression y = 2 + b*0                     |
          |                                                 |
    Y  2.0 +-SOLUTION---------------------------X-(3,2)------+
          |                                                 |
          |                                                 |
      1.5 +                                                 +
          |                                                 |
          |                                                 |
      1.0 +            X (2,1)                              +
          |                                                 |
          |  Final MLE equations to solve thes two          |
      0.5 +  0=b-7*a/17 - 14/17  a=2                        +
          |  0=a-7*b/3 + 2       if a=2 then b=0            |
          |                                                 |
          |  y = 2 + b*0                                    |
          |                                                 |
          -+-----------+-----------+-----------+-----------+-
          1.5         2.0         2.5         3.0         3.5

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   Find a and b using MLE                                                                                               */
    /*                                                                                                                        */
    /*   y = a - b*x                                                                                                          */
    /*                                                                                                                        */
    /*    X    Y                                                                                                              */
    /*                                                                                                                        */
    /*    2    1                                                                                                              */
    /*    3    2                                                                                                              */
    /*    2    3                                                                                                              */
    /*                                                                                                                        */
    /*  Objective finction get extrema                                                                                        */
    /*      2                                                                                                                 */
    /*     ---                                                                                                                */
    /*     \                     2                                                                                            */
    /*   -  .   (yi - a - b * xi)                                                                                             */
    /*     /                                                                                                                  */
    /*     ---                                                                                                                */
    /*      0                                                                                                                 */
    /**************************************************************************************************************************/

    /*   _
     ___| |_ ___ _ __  ___
    / __| __/ _ \ `_ \/ __|
    \__ \ ||  __/ |_) \__ \
    |___/\__\___| .__/|___/
                |_|
    */

    Opjective function

        2
       ---
       \                     2
     -  .   (yi - a - b * xi)    (an upside down parabola? with 2 as an exteema?)
       /
       ---
        0

    Note:  (y - a - b * x)**2  = a**2 + 2*a*b*x - 2*a*y + b**2*x**2 - 2*b*x*y + y**2

    The partial derivative with repsect to a is

    f'(a) = 2*a + 2*b*x - 2*y

    Substituting our input we get

    6*a - 2*b*x[0] - 2*b*x[1] - 2*b*x[2] - 2*y[0] - 2*y[1] - 2*y[2]

    6*a - 2*b*x[0] - 2*b*x[1] - 2*b*x[2] - 2*y[0] - 2*y[1] - 2*y[2]
    6a  =       4b    +    6b   +     4b  +    2   +    4  +   6
    6a  =       14b + 12

    Solving for a we get the equation for a

    a= 7*b/3 + 2

    We do the same for b and get

    b=7*a/17 - 14/17

    Solving the two simultaneous equations we get

    0 = 7*a/17 - 14/17  implies a=2
    0 = 7*b/3 + 2       if a=2 then b=0

    Solution is
     y = 2 +0*b

    /*         _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| `_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    */

    %utl_submit_wps64x("
    proc python;
    submit;
    from sympy import *;

    b = Symbol('b');
    a = Symbol('a');
    x = Symbol('x', integer=True);
    y = Symbol('y', integer=True);
    i = symbols('i', cls=Idx);

    x_values = [2,3,2];
    y_values = [1,2,3];
    n = len(x_values)-1;

    function = summation((Indexed('y',i) - a+b*Indexed('x',i))**2, (i, 0, n));
    partial_intercept = function.diff(a);
    print(partial_intercept);

    intercept_f = lambdify([x, y], partial_intercept);
    inter = solve(intercept_f(x_values, y_values), a);
    print(inter);

    partial_gradient = function.diff(b);
    print(partial_gradient);

    intercept_f = lambdify([x, y], partial_gradient);
    inter2 = solve(intercept_f(x_values, y_values), b);
    print(inter2);

    print(inter2[0]);
    print(inter[0]);

    ans = solve([a-inter[0], b-inter2[0]]);
    print(ans);
    endsubmit;
    ");

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* 6*a - 2*b*x[0] - 2*b*x[1] - 2*b*x[2] - 2*y[0] - 2*y[1] - 2*y[2]                                                        */
    /*                                                                                                                        */
    /* [7*b/3 + 2]                                                                                                            */
    /*                                                                                                                        */
    /* 2*(-a + b*x[0] + y[0])*x[0] + 2*(-a + b*x[1] + y[1])*x[1] + 2*(-a + b*x[2] + y[2])*x[2]                                */
    /*                                                                                                                        */
    /* [7*a/17 - 14/17]                                                                                                       */
    /*                                                                                                                        */
    /* 7*b/3 + 2                                                                                                              */
    /*                                                                                                                        */
    /* {a: 2, b: 0}                                                                                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*     _   _
      ___ | |_| |__   ___ _ __   _ __ ___ _ __   ___  ___
     / _ \| __| `_ \ / _ \ `__| | `__/ _ \ `_ \ / _ \/ __|
    | (_) | |_| | | |  __/ |    | | |  __/ |_) | (_) \__ \
     \___/ \__|_| |_|\___|_|    |_|  \___| .__/ \___/|___/
                                         |_|
    */

    https://github.com/rogerjdeangelis/utl-calculating-the-cube-root-of-minus-one-with-drop-down-to-python-symbolic-math-sympy
    https://github.com/rogerjdeangelis/utl-solve-a-system-of-simutaneous-equations-r-python-sympy
    https://github.com/rogerjdeangelis/utl-symbolic-algebraic-simplification-of-a-polynomial-expressions-sympy
    https://github.com/rogerjdeangelis/utl-sympy-technique-for-symbolic-integUation-of-bivariate-density-function

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

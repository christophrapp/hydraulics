Hydraulics for engineers - a course with experiments and open source codes
Christoph Rapp

The Octave programs have been published under the GNU General Public License (http://www.gnu.org/licenses). All entries and calculations are made in SI units. The programs partially access the input files saved in .csv format. All files must be located in one directory and accessed from there.
Quick Start

Call up the following scripts for each topic

    Open channel hydraulics: normalwater.m
    Pipe hydraulics: couette.m
    transient pipe hydraulics: waterHammer.m
    Potential theory: potential.m

General functions

Function name: dataInput(daten,delimiter,header)

    Description: Assigns the values separated by the 'delimiter' to the data in the file 'daten.XXX'. The lines defined with the integer number header are skipped
    Output: all variables defined individually in the file are assigned their value

Function name: readData(file,delimiter)

    Description: assigns the values separated by the 'delimiter' to the data in the file 'file.XXX'.
    Output: all variables defined individually in the file are assigned their value
    Function name: A=area(qs,qsP,y)

    Description: Calculation of the cross-sectional area for circular, trapezoidal or parabolic profiles
    Input values: Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow depth: y
    Result: Cross-sectional area: A

Function name: P=perimeter(qs,qsP,y)

    Description: Calculation of the wetted perimeter of circular, trapezoidal or parabolic profiles
    Input values: Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow depth: y
    Result: wetted perimeter: U

Function name: bWs=bWS(qs,qsP,y)

    Description: Calculation of the width of the water level for circular, trapezoidal or parabolic profiles
    Input values: Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow depth: y
    Result: Width of the water level: bWs

Function name: sp=balancePoint(qs,qsP,y)

    Description: Calculation of the center of gravity of circular, trapezoidal or parabolic profiles
    Input values: Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow depth: y
    Result: Center of area: sp

Function name: lambda=pc(qs,qsP,y,Q,ks,T)

    Description: Calculation of the pipe friction coefficient lambda using the Prandtl-Colebrook algorithm
    Input values: Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow depth: y (for fully filled pipe y=d) Flow rate: Q Equivalent sand roughness: ks Temperature: T
    Input file:	Line 19: kinViskositaet.csv
    Result: Pipe friction coefficient: lambda

Open channel hydraulics
  
Function name: f=stK(y,qs,qsP,Q,g,rho,StN)

    Description: Solver for support force calculation for conjugate flow depth for alternating jumps
    Input values: Flow depth: y Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow rate: Q Gravitational constant: g Density: rho Supporting force under normal water conditions: StN
    Call:	[yk,fval,info]=fsolve(@(yk) stK(yk,qs,qsP,Q,g,rho,StN),yGuess)
    Result: Function value f for conjugate flow depth: yk

Function name: f=NWC(y,qs,qsP,Q,g,T,ks,kSt,JS)

Description: Solver of the normal wqater conditions
    Input values: Flow depth: y Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow rate: Q Gravitational constant: g Temperature: T Equivalent sand roughness: ks (if ks=0, the roughness is included via the Manning-Strickler equation) Strickler coefficient: kSt (if kSt=0, the roughness is calculated via the Prandtl-Colebrook algorithm) Bottom slope: JS
    Subroutines: (yk,qs,qsP,Q,g,rho,StN)
    Call:	[yk,fval,info]=fsolve(@(yk) stK(yk,qs,qsP,Q,g,rho,StN),yGuess)
    Result: Function value f for normal water flow depth: yN

Function name: yc=yC(qs,qsP,Q,g)

    Description: Calculation of the critical flow depth
    Input values: Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow rate: Q Gravitational constant: g
    Subroutines: phiCcircle % listed below yCtrapezoid % listed below
    Result: critical flow depth: yc

Function name: yk=conjugated(qs,qsP,y,Q,g,rho,yc)

    Description: Calculation of the conjugated flow depth for alternating jumps
    Input values: Cross-section shape: qs('circle','trapezoid','parabola') Cross-section parameters: qsP(d, [s,m],a) Flow depth: y Flow rate: Q Gravitational constant: g Density: rho Critical flow depth: yc
    Subroutines: stK(yk,qs,qsP,Q,g,rho,StN)
    Call:	[yk,fval,info]=fsolve(@(yk) stK(yk,qs,qsP,Q,g,rho,StN),yGuess)
    Result: conjugate flow depth: yk

    Script name: normalwater.m

    Description: Calculation of the normal water conditions.
    Input file:	Line 18: dataOpenChannel.csv
    Screen input:	Input whether the flow depth is to be determined in sub- (yl) or supercritical flow (yr) Enter known flow depth on the right (streaming) or left (shooting)
    Subroutines:	NWV(y,qs,qsP,Q,g,T,ks,kSt,JS) conjugated(qs,qsP,y,Q,g,rho,yc)
    Result/Output: Normal water flow depth: yN Normal water velocity: vN Normal water energy level: HN Normal water Froude number: FrN Critical flow depth: yc Critical velocity: vc Minimum energy level: Hmin Conjugate flow depth: yk Energy level at conjugate flow depth: Hk Froude number at conjugate flow depth: Frk

Script name: directStep.m

    Description: Calculation of the water level at a certain distance Dx upstream (in subcritical) or downstream (in supercritical).
    Input file:	Line 18: dataOpenChannel.csv
    Screen input:	Enter whether the flow depth is to be determined in subcritical (yl) or in supercritical (yr) Enter known flow depth on the right (flow) or left (shoot)
    Subroutines: 1. dataInput.m % assigns the corresponding values to the variables in the input file 2. boessSolve.m % solves the direct step equation iteratively
    Result: Flow depth: y

    Script name: directStepDeltaX.m

    Description: Calculation of the distance between two water levels
    Input file:	Line 18: dataOpenChannel.csv
    Screen input:	Input of the known flow depths on the right and left
    Subroutines: dataInput.m % assigns the corresponding values to the variables in the input file

Script name: sUs.m

    Description: Calculation of the sink or surge height
    Input file:	Line 18: dataSuS.csv
    Screen input:	Input of the original flow depth Input of the change in flow (note the sign! Reduction (-), increase (+))
    Subroutines: datenEinlesen.m % Assigns the corresponding values to the variables in the input file bWsp.m % Calculation of the width of the water level
    Output: Height of the dam wall or stoplog: h

Pipe hydraulics
Script name: couette.m

    Description: Calculation of the velocity profile of a Couette-Poiseuille flow.
    Screen inputs:	Channel width: B Pressure gradient: dp/dx Velocity of the upper plate uB Enter the known flow depth on the right (flowing) or left (shooting)
    Output: Velocity profile.png in the current folder

Function name: f=energy(Q,qs,qsP,y,l,ks,sZeta,A0,At,mue,g,T,dH)

    Call:	[Q,fval,info]=fsolve(@(Q),qs,qsP,y,l, ks,sZeta,A0,At,mue,g,T,dH,PT,PP,eta),guess)
    Description: Calculation of the flow in a pipe system with the following parameters: Cross-sectional shape: qs('circle','trapezoid','parabola') Cross-sectional parameters: qsP(d, [s,m],a) Flow depth: y Length: l Equivalent sand roughness: ks Sum of the individual losses (don't forget the outlet loss!): sZeta Gravitational constant: g Temperature: T Water level difference between inlet and outlet: dH Pump power: PP Turbine power: PT Efficiency: eta
    Subroutines: 1. area.m % calculates the cross-sectional area 2. circumference.m % calculates the wetted circumference
    Result: Flow rate: Q

Function name: f=rohr(Q,qs,qsP,y,l,ks,sZeta,A0,At,mue,g,T,dH,PT,PP,eta)

    Call:	[Q,fval,info]=fsolve(@(Q),qs,qsP,y,l, ks,sZeta,A0,At,mue,g,T,dH,PT,PP,eta),guess)
    Description: Calculation of the flow in a pipe system with the following parameters: Cross-sectional shape: qs('circle','trapezoid','parabola') Cross-sectional parameters: qsP(d, [s,m],a) Flow depth: y Length: l Equivalent sand roughness: ks Sum of the individual losses (don't forget the outlet loss!): sZeta Gravitational constant: g Temperature: T Water level difference between inlet and outlet: dH Pump power: PP Turbine power: PT Efficiency: eta
    Subroutines: 1. area.m % calculates the cross-sectional area 2. circumference.m % calculates the wetted circumference
    Result: Flow rate: Q

Unsteady pipe hydraulics
Script name: waterHammer.m

 Description: Program for calculating the velocities and pressures of unsteady pipe flows using the characteristics method.
    Screen inputs: pipe flow: Display of the piezometric pressure head (piezo), the pressure surge head (only) or the pressure head (DL) A0new: A0 is the pipe cross-section as default. If this is already reduced by a valve in the stationary state, the cross-sectional area must be entered here. Otherwise, confirm with Enter. closureType: Type of closure set (hyperbolic, linear) aNew: Enables correction of the propagation velocity a
    Input file:	Line 26: kinViskositaet.csv Line 27: vaporpressure.csv
    Subroutines: 1. area.m % calculates the cross-sectional area 2. circumference.m % calculates the wetted circumference 3. energy.m % calculates the flow rate 4. close.m % calculates the area of the closing element at time t
    Result: Matrices of the time and location-dependent variables: Velocity: v Pressure head: h
    Output: druckstossBsp.png in the current folder druckstossBsp.pdf in the current folder

Function name: At=closing(A0,ts,t,gap,closureLaw)

    Description: Calculation of the open area of the closing element at time t with the parameters: original opening area: A0 closing time: ts time: t at complete closure remaining opening area: gap closure type: closingLaw (linear,hyperbolic)
    Result: Opening area of the closure organ at time t: At

Function name: v=vValve(h,k,mue,At,A,a,g,node)

    Description: Calculation of the flow velocity at the valve with the following parameters: Pressure head: h Constant from the previous time step of the neighboring node: k (km or kp) Loss coefficient of the valve: mue Opening area at time t: At Cross-sectional area of the pipe: A Propagation velocity of the disturbance: a Gravitational constant: g Location of the closure organ: ort ('top' or 'bottom')
    Result: Opening area of the valve at time t: At

Potential theory
Function name: [phi,psi,u,v]=parallel(x,y,u0,v0)

    Description: Calculation of the potential (phi) and stream function (psi) as well as the velocity components u and v in a field defined by x and y, which is subject to a parallel flow with u0 and v0.
    Input values: Matrix of the field: x in the form [x1,x2,xn;x1,x2,xn;x1,x2,xn] Matrix of the field: y in the form [y1,y1,y1;y2,y2,y2;yn,yn,yn] constant velocity in x-direction: u0 constant velocity in y-direction: v0
    Result: Potential function in the field: phi Current function in the field: psi Velocity component in x-direction: u Velocity component in y-direction: v

    Function name: [phi,psi,u,v]=source(x,y,posX,posY,Q)

    Description: Calculation of the potential (phi) and stream function (psi) as well as the velocity components u and v in a field defined by x and y, which has a source (+) or a sink (-) with the specific flow Q at the point (posX,poxY).
    Input values: Matrix of the field: x in the form [x1,x2,xn;x1,x2,xn;x1,x2,xn] Matrix of the field: y in the form [y1,y1,y1;y2,y2,y2;yn,yn,yn] x-position of the dipole: posX y-position of the dipole: posY Dipole strength: m
    Result: Potential function in the field: phi Current function in the field: psi Velocity component in x-direction: u Velocity component in y-direction: v

Function name: [phi,psi,u,v]=dipol(x,y,posX,posY,m)

    Description: Calculation of the potential (phi) and stream function (psi) as well as the velocity components u and v in a field defined by x and y, which has a dipole with the strength m at the point (posX,poxY).
    Input values: Matrix of the field: x in the form [x1,x2,xn;x1,x2,xn;x1,x2,xn] Matrix of the field: y in the form [y1,y1,y1;y2,y2,y2;yn,yn,yn] x-position of the dipole: posX y-position of the dipole: posY Dipole strength: m
    Result: Potential function in the field: phi Current function in the field: psi Velocity component in x-direction: u Velocity component in y-direction: v
    
Function name: [phi,psi,u,v]=vortex(x,y,posX,posY,gamma)

    Description: Calculation of the potential (phi) and srteam function (psi) as well as the velocity components u and v in a field defined by x and y, which has a potential vortex with the strength gamma at the point (posX,poxY).
    Input values: Matrix of the field: x in the form [x1,x2,xn;x1,x2,xn;x1,x2,xn] Matrix of the field: y in the form [y1,y1,y1;y2,y2,y2;yn,yn,yn] x-position of the vortex: posX y-position of the vortex: posY Vortex strength: m
    Result: Potential function in the field: phi Current function in the field: psi Velocity component in x-direction: u Velocity component in y-direction: v

Script name: potential.m

  Description: Program for calculating potential flows. The program searches the folder for the files of the elementary solutions source.csv, parallel.csv, vortex.csv and dipol.csv and includes the input data if necessary.
    Input file:	Line 28: source.csv Line 35: dipol.csv Line 42: vortex.csv Line 49: parallel.csv
    Subroutines: readData.m % assigns the corresponding values to the variables plotStreamline.m % draws the current and equipotential lines
    Result: Potential function: phi Current function: psi Velocity field in x-direction: u Velocity field in y-direction: v
    Output: streamL.png in the current folder

Function name: stream=plotStreamline(x,y,u,v,dx,dy,seed)

    Description: Program for calculating streamlines that start at the seed points seed.
    Input values: Matrix of the field: x in the form [x1,x2,xn;x1,x2,xn;x1,x2,xn] Matrix of the field: y in the form [y1,y1,y1;y2,y2,y2;ym,ym,ym] Matrix of the field: u in the form [u11,u12,u1n;u21,u22,u2n;um1,um2,umn] Matrix of the field: v in the form [v11,v12,v1n;v21,v22,v2n;vm1,vm2,vmn] equidistant node spacing in x-direction: dx equidistant node spacing in y-direction: dy Seed point of the streamlines: seed in the form [x1,y1;x2,y2;x3,y3;xn,yn]
    Result: streamlines of the form stream{i}(x,y)

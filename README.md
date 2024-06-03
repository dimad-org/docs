### SLAC REPORT 285 UC-28 (A) May 1985

# USERS GUIDE TO THE PROGRAM DIMAD(\*)

Roger V. SERVRANCKX

### University of Saskatchewan, Saskatoon Canada and Stanford Linear Accelerator Center, Stanford California USA 

# Karl L. BROWN

Stanford Linear Accelerator Center,  
Stanford California USA

# Lindsay SCHACHINGER

### SSC CDG, Lawrence Berkeley Lab, Berkeley, California USA 

# David DOUGLAS

### CEBAF project. SURA, Newport News Virginia USA 

<span class="small">(\*) Work sponsored by the Department Of Energy and
by the National Science and Engineering Research Council of
Canada</span>

------------------------------------------------------------------------

The present guide corresponds to the program version dated JULY 14 1989.

The program DIMAD studies particle behaviour in circular machines and in
beam lines.

The trajectories of the particles are computed according to the second
order matrix formalism<sup>([1](#references))</sup>. It does not provide
synchrotron motion analysis but can simulate it. The program provides
the user with the possibility of defining arbitrary elements to tailor
the program to specific uses. The present version of DIMAD is not fully
debugged. Please inform one of the following persons about any anomalies
observed:

                   David Douglas at CEBAF 804/249 7512
                   Lindsay Schachinger at LBL 415 486 6590
                   Roger Servranckx at Saskatoon 306 966 6054

------------------------------------------------------------------------

# \[[index](#index)\]

------------------------------------------------------------------------

<span id="introduction"></span>

## INTRODUCTION

DIMAD,like its predecessor DIMAT, is the result of many years of
experimenting with several different charged particle computer codes.

In 1970 the first author had the good fortune of discovering the program
OSECO (Optique du SECond Ordre) written by J.L.LACLARE at SACLAY.
Basically OSECO was a second order Tracking program. It was based on the
second order matrix formalism of TRANSPORT and was originally written
for a CDC computer. Its usefulness in the simulation of the extraction
procedure of the Beam Stretcher ALIS and later of EROS led to the desire
for a program that would have more analysis power. The first attempt to
develop a new program resulted in the program DEPART which was written
as a pure differential equation ray tracing program but it soon became
clear that DEPART was very awkward to use because of the cumbersome way
in which bending magnets were defined in the code. An evolutionary
process then took place over a period of several years finally resulting
in the present program called DIMAD.

Many people contributed in various ways to the development of DIMAT. Dr
Leon Katz, while he was Director of the Linear Accelerator Laboratory at
Saskatoon, provided strong support for the work. Sheila Flory, Dean
Jones, Edward Pokraka, Jim Morrison, and Jean Mary Miketinac provided
programming support at different times during the initial program
development. Ideas were borrowed freely from the program OSECO and Jean
Louis Laclare helped formulate some of the early developments. Karl L
Brown of SLAC became influential during the later development phases. He
helped formulate the more recent contributions to the program (geometric
aberrations, linear analysis of motion around arbitrary reference
orbits, and magnet misalignment simulations). It is for these reasons
that he has become a coauthor of the present manual.

The authors wish to thank the many DIMAT users of other laboratories for
their comments and assistance in locating the many programming errors
that have occurred during the evolution of DIMAD.

<span id="introduction">In 1984, it became clear that tracking codes
should operate in a canonical environment ,should provide options for
symplectic tracking and should conform to the input
STANDARD<sup>(</sup></span>[2](#references)).

Adapting the input code of MAD<sup>([3](#references))</sup>, Lindsay
Schachinger transformed the program so it would enjoy a common input
with MAD, thereby conforming to the input STANDARD.

With ideas developed originally by
E.Forest<sup>([4](#references))</sup>, David Douglas introduced the
symplectic tracking options and the canonical variables.

------------------------------------------------------------------------

<span id="element_and_machine_data_input"></span>

<span id="element_and_machine_data_input"></span>

## <span id="element_and_machine_data_input">ELEMENT and MACHINE DATA INPUT</span>

<span id="element_and_machine_data_input">The input format to dimad now
conforms quite closely to the standard format, as laid out in references
2. This conversion of dimad to standard input was accomplished by taking
the input subroutines from the program MAD (Reference 3) and making from
these routines (with modifications) an input interface for dimad.</span>

One exception to the standard format is the units conventions.

<span id="element_and_machine_data_input">Input to dimad can be in
either transport units (indicated by the keyword "utransport"), or in
standard units (indicated by the keyword "ustandard"). For more
information on units, see the next section. The second difference
between dimad and the standard is the addition of several keywords for
dimad. The added keywords are "quadsext", "gkick", and "mtwiss." These
elements are described more fully later. Also, in dimad, the solenoid
can have a quadrupole field. Elements which are described in
references</span> [2](#references), but which are not implemented in
dimad are separator and rbend.

The job title entered on a line following one with the keyword "title."
This should be followed with a units keyword. If no units keyword is
found, the units are assumed to be the standard units.

------------------------------------------------------------------------

<span id="units"></span>

## <span id="units">UNITS</span>

<span id="units">The keyword "utransport" indicates that the input has
the following units</span>

angles in degrees except for dx' and dy' for the kicks.

lengths in meters

energy in Gev

electromotive force in kilovolts

frequency in Hz

field expansion is B(x,0) = Brho SUM Kn\*x\*\*n.

positive K1 is horizontally focussing.

Also, the "utransport" keyword has implications for the field expansion
coefficients in the "sbend" element. The keyword ustandard indicates
that the input has the following units

angles in radians

lengths in meters

energy in Gev

electromotive force in megavolts

frequency in Megahertz

field expansion is B(x,0) = Brho SUM Kn\*x\*\*n/n!

positive K1 is horizontally focussing.

------------------------------------------------------------------------

<span id="units"></span> <span id="general_syntax"></span>

## <span id="general_syntax">GENERAL SYNTAX</span>

<span id="general_syntax">When describing the machine, a statement can
be continued on the next line by ending the current line with a "&". A
comment line begins with a "!". Any line containing one of the
characters "\*","(","@" in the first column is treated as a
comment.</span>

A ";" is used to separate statements on the same line. Keywords are
uniquely specified by the first four letters, and only those four must
be entered. At most 8 letters in a keyword are checked.

The keyword NOECHO can be used to suppress transmission of the input
data stream to the output files. The keyword ECHO reinstates the stream
of the input data to the output files.

------------------------------------------------------------------------

<span id="general_syntax"></span> <span id="parameters"></span>

## <span id="parameters">PARAMETERS</span>

<span id="parameters">Parameters are defined with a statement
like</span>

                 pname = value

where pname is any parameter name. Parameters can then be used in
element definitions. Value can also be an arithmetic expression
involving other parameters. Throughout the element definitions, a
parameter value can also be an arithmetic expression. Note that in dimad
the relationships between parameters are lost, but during the machine
definition phase they are treated correctly.

Examples:

                 lslot=100
                 lb=lslot/8
                 lh = sqrt(lslot)

**Note**: the value of PI if needed must be defined as an input
parameter. the value halfturn must be understood as either PI radians or
as 180 degrees depending on the units chosen.

------------------------------------------------------------------------

<span id="parameters"></span> <span id="element_definitions"></span>

<span id="element_definitions"></span>

## <span id="element_definitions">ELEMENT DEFINITIONS</span>

<span id="element_definitions">To define an element,</span>

                 label: type [,pkeyw=value,......]

where label is the name of the element, type is an element type (see
below) and pkeyw is a parameter keyword appropriate for the element type
(see below). value again can be a parameter name, or an expression
involving parameters. Examples:

                 b: sbend, l=lb, angle=lb/rho
                 d0: drift

------------------------------------------------------------------------

<span id="element_definitions"></span> <span id="elements"></span>

<span id="elements"></span>

## <span id="elements">ELEMENTS</span>

<span id="elements">A list of all element types and the relevant
parameter keywords follows. Unless otherwise noted, all values default
to zero except the aperture, which defaults to 1 meter.</span>

      drift
             l        is the length.

      sbend
             l        is the length.
             angle    is the bend angle.
             k1       If the standard convention is being used, k1 is given
                      by  the field  expansion in  the  UNITS section.   If
                      transport  conventions are  being used,   k1  is n  =
                      -rho(dBo/dx)/Bo.
             e1       is the entrance edge angle.
             e2       is the exit edge angle.
             tilt     is the tilt angle.  If tilt is entered with no value,
                      halfturn/2 is assumed.
             k2       If the standard convention is being used, k2 is given
                      by  the field  expansion in  the  UNITS section.   If
                      transport  conventions   are  being   used,   k2   is
                      beta=rho**2(d2Bo/dx2)/(2Bo).
             h1       is the entrance pole face curvature.
             h2       is the exit pole face curvature.
             hgap     is the entrance half gap size.  If hgapx is not given
                      a value,  it  defaults to the value  of hgap.  During
                      fitting, however,  both hgap and hgapx must be varied
                      together.
             fint     is the entrance fringe field integral, which defaults
                      to 0.5.   If fintx is not given a value,  it defaults
                      to the value of fint.  During fitting, however,  both
                      fint and fintx must be varied together.
                              hgapx    is the exit half gap size. See hgap.
                              fintx    is  the exit fringe  field integral.
                      See fint.

      rbend 

The rbend is a parallel faced dipole magnet. Its parameters are the same
as those of the sbend. Parameters e1 and e2 are not provided by the user
and are set by the program to half the value of the bend angle.

     quadrupole
             l        is the length.
             k1       is the strength.
             tilt     is the tilt angle.  If tilt is entered with no value,
                      halfturn/4 is assumed.
             aperture is the magnet aperture for the Hardware operation.

     sextupole
             l        is the length.
             k2       is the strength.
             tilt     is the tilt angle.  If tilt is entered with no value,
                      halfturn/6 is assumed.
             aperture is the magnet aperture for the Hardware operation.

     quadsext
             l        is the length.
             k1       is the quadrupole strength.
             k2       is the sextupole strength.
             tilt     is the tilt angle.  If tilt is entered with no value,
                      halfturn/4 is assumed.
             aperture is the magnet aperture for the Hardware operation.
     
     octupole
             l        is the strength.
             k3       is the strength.
             tilt     is the tilt angle.  If tilt is entered with no value,
                      halfturn/8 is assumed.
             aperture is the magnet aperture for the Hardware operation.

     multipole
             l        is the length.  If the length is zero,  the strengths
                      are interpreted as integrated strengths.
             k0 - k20 are the strengths.
             t0 - t20 are  the tilt  angles.  If  tn is  entered without  a
                      value, halfturn/2(n+1) is assumed.
             scalefac is a dimensionless strength factor, used to scale all
                      the strengths together.
             tilt     is the overall tilt angle.
             aperture is the magnet aperture for the Hardware operation.

**NOTE1** : only the components with non zero amplitude are stored! If
zero components need be kept for the purpose of generating errors via
the ERROR definition then enter components with small amplitudes.

**NOTE2** : when a quadrupole or a sextupole component is present the
matrix of this component is computed for half the length of the
multipole. This does not change the value of the total second order
matrix. During tracking operations the particles are tracked through
half the element as quadrupole or sextupole then the higher order
multipole kicks are applied and the particles are tracked through the
second half of the quadrupole or sextupole component. This feature is
important in computing misalignment effects with multipole components
present.

     solenoid
             l        is the length.
             ks       is the solenoid strength. ks=0.5*Bs/Brho.
             k1       is the quadrupole strength.
             tilt     is the tilt angle.  If tilt is entered with no value,
                      halfturn/4 is assumed.
             aperture is the magnet aperture for the Hardware operation.

     rfcavity
             l        is the length.
             volt     is the cavity voltage.(kV for Utransport)
             lag      is the  phase lag  of the  cavity with  respect to  a
                      nominal particle  (0,0,0,0,0,0)  at the start  of the
                      machine.(Degrees for Utransport)
             freq     is the frequency of the cavity.(Hz for Utransport) In
                      program  versions dated  July 14  1989  or later  the
                      frequency is  defined via the harmonic  number.  This
                      has the  advantage to match  the frequency  with full
                      accuracy to  the length  of the  machine.   When  the
                      frequency must be not matched a non integer value for
                      the harmonic number is entered. Note that the keyword
                      has  remained  FREQ  (until   further  notice).   The
                      particle type (electrons or protons)  is selected via
                      the constant definition operation.
             energy   is the energy.(GeV)

     roll
      This element performs  a rotation of the coordinate  system about the
      longitudinal axis. 
             angle    is the rotation angle. A positive angle means the new
                      coordinate system is  rotated clockwise about  the s-
                      axis with respect to the old system.

     zrot 
      This element performs  a rotation of the coordinate  system about the
      vertical axis. The angle must be small. 
             angle    is the rotation angle. A positive angle means the new
                      coordinate  system  is rotated  clockwise  about  the
                      local z-axis with respect to the old system.

     hkick, vkick 
      These  elements are  translated  by the  program  into general  kicks
      (gkick). 
             kick     a horizontal (vertical) kick of size kick,
             angle    about the longitudinal axis.

     gkick
      This element is a general kick.
             l        is the length.
             dx       is the change in x.
             dxp      is the change in x'.
             dy       is the change in y.
             dyp      is the change in y'.
             dl       is the change in path length.
             dp       is the change in dp/p.
             angle    is  the  angle  through  which  the  coordinates  are
                      rotated about the longitudinal axis.
             dz       is the longitudinal displacement.
             v        is the  extrance-exit parameter  of the  kick.  v  is
                      positive for an  entrance kick,  and negative  for an
                      exit kick.   The absolute value of v is used to force
                      the  kick to  be applied  every  abs(v)  turns.   The
                      default value of v is 1.
             t        is the momentum dependence  parameter.  The kicks dx'
                      and dy' can  be thought of as  misalignment errors or
                      as angle kicks of orbit correctors. In the first case
                      (t=0)  they  are momentum independent.  When  t=1 the
                      kicks  dx'  and  dy' vary  inversely  with  momentum.
                      When t is set to a negative integer value -n the kick
                      is applied every turn and  the momentum of a particle
                      with  initial momentum  p will  oscillate around  the
                      nominal  momentum p0  with  amplitude  (p-p0)  and  a
                      period equal to n turns.  More than one such kick may
                      be put in the line  (all identical though)  the phase
                      of  the cosine  oscillation  is  proportional to  the
                      pathlength of the reference trajectory.

     hmon, vmon, monitor
      These elements are horizontal, vertical,  and horizontal and vertical
      monitors, respectively.
             l        is the monitor length.
             xserr,
             yserr,
             xrerr,
             yrerr    are the x and y systematic and random errors.

**NOTE**: the errors are not used in this form presently. Errors are
introduced via the misalignement operations.

     marker
      A marker is a drift element of zero length. It has no parameters.

     ecollimator, rcollimator
      An ecollimator is elliptic,  and an rcollimator is rectangular.   The
      particles  are  checked  at  the  entrance and  at  the  exit  of  the

     collimator.
             l        is the length.
             xsize
             ysize    are the  x and y  collimator apertures.   The default
                      apertures are 1 meter.

     arbitelm
      This is the arbitrary element.  Its parameters  are used in the user-
      supplied  routine  TRAFCT,   which   contains  the  transfer  function
      describing  the  effect  of  arbitrary   elements  on  the  individual
      particles.  All arbitrary elements use  the same subroutine.  Distinct
      arbitrary elements can  only be recognized by the  program through the
      use of one parameter as a flag.
             l        is the length.
             p1 - p20 are the parameters.

     mtwiss
             l        is the length.
             mux,
             betax,
             alphax,
             muy,
             betay,
             alphay   are the  twiss parameters  for this  transfer matrix.
                      betax and betay have default values of 1.
     matrix
      This element is a general transfer matrix.
             rij
             tijk     are the matrix elements.  i,j,  and k range from 1 to
                      6, but j is always less than or equal to k.

------------------------------------------------------------------------

<span id="elements"></span><span id="beamline_definitions"></span>

<span id="beamline_definitions"></span>

## <span id="beamline_definitions">BEAMLINE DEFINITIONS</span>

<span id="beamline_definitions">A beamline is a list of elements, which
can include other beamlines.</span>

             label: line=(member1, member2, member3,.....)

denotes a beamline called label. The members can be elements, other
beamlines, sequences of members, or any of the above preceeded by a
repetition count and/or a minus sign for reflection.

Examples are

             df:line = (dq, oo, b, oo qf)
             fdstar:line = (qf, sf, b, sd, qd)
             arc:line = (df, 64*(fdstar,df))

Beamlines can also have formal arguments. An example is

             fdstar(sf,sd):line = (qf, sf, b, sd, qd)

where sf and sd are not defined elements, but variables. So

             super:line = (fdstar(sd1,sf1),df,fdstar(sd2,sf2))

is a line in which the elements sd1, sd2, sf1, and sf2 are substituted
for the variables sd and sf in the original definition.

------------------------------------------------------------------------

<span id="beamline_definitions"></span><span id="control_flow"></span>

<span id="control_flow"></span>

## <span id="control_flow">CONTROL FLOW</span>

<span id="control_flow">Beamline definitions are followed by a use
statement in the form</span>

             use, beamlinename.

This causes the beamline beamlinename to be the current machine for
dimad.

Next comes the statement

            dimat

which passes control to dimad, after translating the machine into the
correct data structures for dimad. Any dimad command can then be issued.

A ';' will cause dimad to stop and return control to the input
interface. Now the user can define a new machine and then go back to
dimad and do a new calculation, or stop execution with the command stop.
The use command causes the old machine to be replaced by a new one. This
new machine can be a previously defined beamline. For debugging
purposes, the dump command from MAD has been retained. This command
produces a dump of the MAD-type data structure describing the machine.
After a ";" and return to MAD control, one can specify the use of a new
line , keeping the previously defined (and perhaps modified by dimad)
elements.To do so one uses the commands :

      use,newlinename
      newbeam

The last command "newbeam" passes control back to dimad. Observe that
without newbeam all the element parameters are redefined to their
initial input values.

A new MAD command is introduced : EXPLODE . Its purpose is to provide an
explicit description of the beamline used.

------------------------------------------------------------------------

<span id="control_flow"></span><span id="operation_list_description"></span>

<span id="operation_list_description"></span>

## <span id="operation_list_description">OPERATION LIST DESCRIPTION</span>

<span id="operation_list_description">Each array specifying an operation
starts with a title line of 80 characters or less. The first four non
blank characters (capitalized in the following presentation) specify the
operation and **MAY NOT BE ALTERED**.</span>

The lines following the title may have 72 characters.

Any line containing one of the characters "!","\*","(","@" in the first
column is treated as a comment line.

Each array terminates with a ',' or a ';'. In the first case another
operation is expected,in the second case control is returned to the
interface program.If the user desires to stop the run at this point, the
line following the ';' must contain the MAD command 'STOP'.

In all tracking operations the particle coordinates are checked at the
entrance of some element. Particles are lost when the square of the
radial excursion is greater than the expulsion factor. It is set at the
default value of 1. Its value can be changed via the constant definition
operation.

------------------------------------------------------------------------

<span id="operation_list_description"></span><span id="implemented_operations"></span>

<span id="implemented_operations"></span>

## <span id="implemented_operations">IMPLEMENTED OPERATIONS</span>

<span id="implemented_operations"></span>

     
             ADIABATIC VARIATIONS
             BEAM MATRIX TRACKING
             CONSTANT DEFINITION
             DETAILED CHROMATIC ANALYSIS
             GENERATION OF PARTICLES
             GEOMETRIC ABERRATIONS
             HARDWARE VALUES LISTING OF MACHINE
             INTERACTIVE MANIPULATION OF LATTICE
             LEAST SQUARE FIT
             LINE GEOMETRIC ABERRATIONS
             MACHINE AND BEAM PARAMETERS COMPUTATIONS
             MATRIX COMPUTATION
             MODIFICATION OF ELEMENT DATA
             MOVEMENT ANALYSIS
             OUTPUT CONTROL
             PARTICLE DISTRIBUTION ANALYSIS
             PRINT SELECTION
             PROGRAM GENERATION
             RMATRIX COMPUTATION (6X6)
             SEISMIC PERTURBATION SIMULATION
             SET FIT POINT
             SET LIMITS TO VARIABLES
             SET SYMPLECTIC OPTION ON
             SHO VALUES OF CONSTANTS
             SIMPLE FITTING
             SPACE CHARGE COMPUTATION
             TRACKING OF PARTICLES

               OPERATIONS ASSOCIATED WITH MISALIGNMENTS AND ERRORS
     
             ALIGNMENT FITTING
             BASELINE DEFINITION
             BLOCK MISALIGNMENT
             CORRECTOR DATA DEFINITION
             ERRORS DATA DEFINITION
             MISALIGNMENT DATA DEFINITION
             REFERENCE ORBIT DISPLAY
             SEED
             SET CORRECTOR VALUES
             SET ERRORS OF ELEMENTS
             SET MISALIGNMENT OF ELEMENTS
             SHO MISALIGNMENTS
             SHO ERRORS
             SYNCHROTRON RADIATION DATA DEFINITION

------------------------------------------------------------------------

<span id="use_and_description_of_each"></span>

<span id="adiabatic_variations"></span>

## <span id="adiabatic_variations">USE AND DESCRIPTION OF EACH OPERATION</span>

ADIABATIC VARIATIONS

<span id="adiabatic_variations">This operation enables to vary
parameters of elements during particle tracking operations.At the
present stage this operation destroys the original value of the
parameters varied and so cannot be used in fitting or repeatedly in the
in the same job.Two options are available : linear and sinusoidal
variation.</span>

Input format:

        ADIAbatic variations of some parameters(up to 80 char)
        name pkeyw nopt p1 p2  val1 val2 val3 val4
        ........
        name pkeyw nopt p1 p2  val1 val2 val3 val4
        99,
     
     Parameters:
        name       name of element having a parameter to be varied

        pkeyw      keyword of parameter to be varied (i.e. k1 for a quad)

        nopt       option number
                    1  means variation  will  be  linear according  to  the
                    following  rule:   the  parameter  remains constant  at
                    value  val1  until  turn p1  then  varies  linearly  to
                    achieve the value val2 at turn  p2.   In this case only
                    two parameter p's and only  two values vali are present
                    in the input format
                    2 means the  variation will be sinusoidal  between turn
                    p1 and turn  p2.The variation is done  according to the
                    formula :
                     value = val1 + val2*sin((2pi*turn/val3)+val4)
                    where  turn  is the  current  turn  at which  value  is
                    applied.  Outside turns p1 and p2 the original value is
                    applied.

------------------------------------------------------------------------

<span id="beam_matrix_tracking"></span>

## BEAM MATRIX TRACKING

Computes beam matrices at selected points of the machine from the
initial beam matrix defined in the input of the operation. If sigi and
sigo denote the beam sigma matrices at the entrance and exit of a beam
line section then sigo = R\*sigi\*Rt where R and Rt are the
transformation matrix of the section and its transpose.

Input format:

                       BEAM matrix tracking computations..(up to 80 char)
                       sigx rxx'  rxy  rxy'  rxl  rxp
                            sigx' rx'y rx'y' rx'l rx'p
                                  sigy ryy'  ryl  ryp
                                       sigy' ry'l ry'p
                                             sigl rlp
                                                  sigp
                       mprint [list]
                     or
                       0
                       betax alphax etax etapx epsilonx
                       betay alphay etay etapy epsilony
                       sigl sigp
                       mprint [list]
                     or
                       0
                       0 0 0 0 epsilonx
                       0 0 0 0 epsilony
                       sigl sigp
                       mprint [mlist]

                    Parameters:

        sig*       sigma extension of the  beam.(as defined in reference(1)
                    and reference (5))

        rij         correlation cosines as defined in reference (1).
       betax,alphax,etax,etapx,betay,alphay,etay,etapy :  initial values of
                    twiss parameters used to define an uncoupled beam.

        epsilonx,epsilony  emittances in x and y of the input beam.

        NOTE: when betax  etc  are zero  the  values  are obtained  from  a
                    previous movement  analysis calculation  made within  a
                    MATRix operation  or a Fit  operation that  generates a
                    matrix calculation.

        mprint  -2  no computation is done.    The operation serves only to
                    define a beam as needed  in the operations BEAM tracing
                    and DETAiled analysis with parameter nvh=1.
                -1  print final result only.
                 0  print all intermediate and final results.
                 n  n>0 used  with list.   There  are n intervals  in which
                    printing will occur.
        mprint+1000:when 1000 is added to the value of mprint, the printing
                    occurs in the same fashion as above but a table of beam
                    envelopes is printed instead of the full beam matrix.

        list        contains  the beginning  and end  of  all intervals  in
                    which printing  is done.    List is a  set of  pairs of
                    numbers.They are positions in the order list of machine
                    elements) List may contain up to mxlist numbers (set at
                    40 initially)

------------------------------------------------------------------------

<span id="beam_matrix_tracking"></span><span id="constant_definition"></span>

<span id="constant_definition"></span>

## <span id="constant_definition">CONSTANT DEFINITION</span>

<span id="constant_definition">This operation allows the user to
redefine basic constants. The purpose of this operation is to enable
comparison of the computation results with other programs or to update
the values as their accuracies increase.The constants accessible to the
user are : Pi, the velocity of light (in m/sec), the electron mass (or
particle mass) (in GeV), the electron (or particle) radius, the electron
(or particle) charge. The reference relative momentum (dp/p) that is
used in some Taylor expansion with delta as independent variable.Two
parameters used in the least square minimizer routine are also
accessible to the user as well as the expulsion factor. The scale
factors ETAFAC and SIGFAC are also accessible via this operation.</span>

Use the operation SHO Constant to examine the constants.

Input format:

        CONStant definition .....(up to 80 Characters)
        n(1),val(1),....n(p),val(p)

     Parameters :

     n(i)  is the order  number  of  the  ith   constant  to  be  redefined
                    according  to the  following  order  :  pi,velocity  of
                    light,particle      mass,particle       radius,particle
                    charge,reference   energy   used   in   taylor   series
                    expansions,  least square  fit initial tolerance,factor
                    for maximum  function calls  in least  square fit  ,the
                    expulsion factor and the particle type (0 for electrons
                    and 1 for protons,  the  default is 0).  Presently when
                    the  particle type  is changed  the  particle mass  and
                    radius is  NOT changed.    The particle  type selection
                    only  affects the  rfcavity  functions:   the  particle
                    velocity is taken into account  to compute the phase of
                    the particle relative to the RF.

     val(i) new value of the constant n(i).

------------------------------------------------------------------------

<span id="constant_definition"></span>
<span id="detailed_chromatic_analysis"></span>

## <span id="detailed_chromatic_analysis">DETAILED CHROMATIC ANALYSIS</span>

<span id="detailed_chromatic_analysis">Traces particles (2 per plane,
per momentum) to determine the linearized transfer matrix from the
initial point to any related point in the lattice. A twiss function
computation is then done at these points. The initial central particle
position is assumed to be xo xo' yo yo'. **NOTE**: no kicks simulating
synchrotron oscillation (para- meter T \< 0 ) may exist in the lattice.
Results are meaningless in the presence of such kicks.</span>

For each energy ei,particles are generated around the point Po (xo xo'
yo yo') to compute the elements Rij of the matrix describing the linear
motion around the trajectory defined by the point Po.

Input format:

        DETAiled chromatic analysis...(up to 80 characters)
        NH NV NHV
        xo xo' yo yo'
        dx dx' dy dy'
        betax alphax etax etapx
        betay alphay etay etapy
        Nener Ncoef
        e  e  ...e
         1  2     nener
        MLOCAT [LIST]

     Parameters:

        NH      1   only xx' motion is traced and computed.  Betax, alphax,
                    and nux are computed.
                0   xx' motion alone is not analyzed.

        NV      1   only yy' motion is traced and computed.  Betay, alphay,
                    and nuy are computed.
                0   yy' motion alone is not analyzed.

        NHV     1   coupled motion  xx',  yy'  is traced.    The full  beam
                    matrix is computed.
                2   coupled motion  is computed.A  short print  is provided
                    with x  xp y yp betax  alphax betay alphay nux  nuy for
                    all energies requested
                3   in  this  case three  energies  have  to be  defined  :
                    delta0,  delta0  - eps,  delta0  + eps .   This enables
                    computing the basic machine  parameters associated with
                    energy delta0. Choose eps comfortably close to zero for
                    accurate  computation of  the eta  functions.  A  short
                    print  of the  machine  parameters  is provided.    The
                    values of eta  and etap are affected by  a scale factor
                    ETAFAC which  can be  set via  the constant  definition
                    operation.   Its default  value is  1.0.   When set  to
                    1.0e03(say)) the printed values are in mm and mrad.
                4   The  beam  matrix  values  (as   defined  in  the  BEAM
                    operation)  computed  for one energy  are printed  in a
                    convenient  table format.    The values  of the  matrix
                    values  for  the  beam   sigmas  (not  the  correlation
                    coefficients)  are affected by  the scale factor SIGFAC
                    which can be set via the constant defintion operation.
                5   same as three  with the code added in  the first column
                    to facilitate some plotting work

        NOTE:  the input  beam must have been defined previously  in a BEAM
        MATRIX TRACKING operation.
                   0   coupled xx', yy' motion is not analyzed.
                 When NH, NV, and NHV are all zero,  the program prints the
                 centroid positions only.

        dx dx' dy dy'
                    increments at which the off  orbit particles are placed
                    to compute the sx, cx, sy,  cy functions (see reference
                    (1)).

        betax, alphax, etax, etapx, betay, alphay, etay,etapy
                    initial values used in the twiss function computations.

        Nener       number  of energies  for  which  the analysis  is  done
                    (maximum 15).

        Ncoef       number  of  coefficients  used  in  the  Taylor  series
                    expansion as a function of momentum (max 6).

        e  ...e     momentum values in the form (p - p )/p
         1     nener                                  0   0

        MLOCAT      indicates the number of intervals  in which printing is
                    to occur.  If MLOCAT = -1,  then printing occurs at end
                    of lattice only and no number is in list.

        LIST        a set  of pairs of numbers  each of which  indicate the
                    beginning  and  end  position (in  the  order  list  of
                    machine elements)  of each of mlocat intervals in which
                    printing takes place.    List may contain up  to mxlist
                    numbers (set at 40 initially)

------------------------------------------------------------------------

<span id="detailed_chromatic_analysis"></span><span id="generation_of_particles"></span>

<span id="generation_of_particles"></span>

## <span id="generation_of_particles">GENERATION OF PARTICLES</span>

<span id="generation_of_particles">This operation generates a set of
particles to be used subsequently in one or more particle Tracking
operations. Presently only gaussian distributions can be generated. This
operation MUST ALWAYS be preceded by a BEAM definition operation and by
a SEED operation.</span>

Input format:

         GENEration of particles
         nopt sigma1 .... sigma6 scale npart
         x0,x'0,y0,y'0,al0,del0

     Parameters
         nopt  : 1   the particles are randomly generated on the surface of
                    a six  dimensional ellipsoid  (defined previously  by a
                    BEAM operation.   In this case the values of sigmai are
                    not operational (but for  computational efficiency they
                    should be set to 1)
                 3   the particles generated have  coordinates that satisfy
                    a six dimensional gaussian distribution.

     Note: the same value  for the  option parameter  must be  used in  the
                    PARTicle analysis  operation (if  used)  following  the
                    tracking of such particles.
     
         sigmai : the   number  of   sigmas   above   which  the   gaussian
                    distribution is truncated for each of the six variables
                    x,x',y,y',al,delta.   The beam is defined by a previous
                    BEAM operation which is assumed to define the one sigma
                    distribution.

         scale : scales the beam size by the given factor.

         npart : number of particles to be generated. Maximum number mxpart
                    (initially set at 1000)

         x0,x'0,y0,y'0,al0,del0 :   centroid coordinates  around which  the
                    beam is generated.

------------------------------------------------------------------------

<span id="generation_of_particles"></span><span id="geometric_abberations"></span>

<span id="geometric_abberations"></span>

## <span id="geometric_abberations">GEOMETRIC ABERRATIONS IN MULTIPLE TURN OPERATION</span>

<span id="geometric_abberations">This operation traces particles that
are placed on ellipses with nominal emittances epsx(i),epsy(i) for many
turns. It then fits an ellipse to the output points obtained. From this
fitted ellipse it determines the average values for betax, alphax,
betay, alphay, nux, nuy, epsx, and epsy. It also computes the maxima and
minima emittances which informs about the diffusion pattern of the
motion. Variances of the tunes are also computed.</span>

Input format:

                       GEOMetric aberrations .......(up to 80 characters)
                       betax,alphax,betay,alphay
                       xco,xpco,yco,ypco,ener
                       ncase,nturn,njob
                       nplot,nprint
                       epsx   , epsy
                           1        1
                        ....................
                        epsx      ,epsy
                            ncase      ncase
                         anplprt

                    Parameters:

           betax, alphax, betay, alphay
                    input values of the twiss parameters at the entrance of
                    the lattice.   When betax=0  the twiss parameter values
                    are obtained  from a  previously run  movement analysis
                    with nanal not 0. The values corresponding to the first
                    energy are used.   This includes the parameters  xco to
                    ener.

        xco, xpco, yco, ypco, ener
                    coordinates  and momentum  of the  closed orbit  around
                    which the aberrations are to be computed.  When betax=0
                    these parameters are obtained  from a previous movement
                    analysis operation.  Values corresponding  to the first
                    energy are used.

        ncase       number of cases analyzed (maximum 10)

        nturn       number of turns for tracing (maximum 100)

        njob    1   coupled motion analysis is wanted
                2   uncoupled motion analysis is wanted

        nplot   1   plotting of  the resulting  particles.   The  operation
                    always accumulates  the particles at every  nplot turns
                    but plots the  accumulation at the end of  the job.  It
                    also computes its own plotting windows.
               -1   no plotting.

        nprint -2   no printing.
               -1   printing at end of lattice only.
                0   printing after every element.
                n   printing after every n  turns.   Normally nprint should
                    be set = nturn.

        epsx, epsy  ncase values for the chosen nominal
                           i     i emittances in x and y  using the unit
                                   mm-mrad (E-06 m-rad)

        anplprt     parameter selecting the fast fourier transform options.
                    When 0 no fourier transform is performed. 1 the fourier
                    transform components are printed.   10 the amplitude of
                    the  fourier  transform  is  printer-plotted.   100  an
                    analysis of the  peaks is provided.   A  combination of
                    those values is allowed eg: 111 all three are done.
                     It   is   advised   to  trace   for   at   least   500
                    turns,preferably 1000.The  number of turns  should have
                    as many low valued factors  as possible to benefit from
                    the speed of the fast fourier transform.
                     Only the first case of the geometric aberration run is
                    fourier analysed.

------------------------------------------------------------------------

<span id="geometric_abberations"></span><span id="hardware_values_listing_of_machine"></span>

<span id="hardware_values_listing_of_machine"></span>

## <span id="hardware_values_listing_of_machine">HARDWARE VALUES LISTING OF MACHINE</span>

<span id="hardware_values_listing_of_machine">Computes the geometry of
the lattice and parameters related to the strengths and fields of the
magnetic elements.</span>

**NOTE**: presently this operation works only with Transport units. The
run must have started with the command UTRANSPORT.

Input format:

                       HARDware layout  and element  parameters..(up to  80 char)
                       E s x y z theta phi psi conv mprint [list]

                    Parameters:

        E           momentum (GeV/c) used for computation of field values.

        s x y z     coordinates  of   starting  point   in  some   absolute
                    reference coordinate  system.  The coordinate s  is the
                    length of arc along the reference trajectory.To justify
                    the choice of the angles theta, phi, and psi,  the axis
                    z should  coincide more or  less with  the longitudinal
                    axis of  the beam.   The  angles theta,  phi,   and psi
                    describe the motion needed to bring the absolute system
                    of reference  in coincidence with  the local  system of
                    coordinates.   The  local system of coordinates  is the
                    system used by  the program.  Its z axis  is tangent to
                    the reference trajectory.  The x axis (uniquely defined
                    by the bends) is in the midplane of symmetry and points
                    outwards of the bend.   The  y axis completes the local
                    right  handed  system  of  reference.    To  bring  the
                    absolute system in coincidence with the local reference
                    system,  one executes the following rotations (strictly
                    in the order indicated):
                       A rotation  theta around the  y axis  (positive when
                       the z axis turns towards the x axis)
                       A rotation phi around the  x axis (positive when the
                       z axis turns towards the y axis: i.e. points upwards
                       for a bend deflecting the beam to the right)
                       A rotation  psi (called sometimes the  roll)  around
                       the z axis  (positive when the x  axis turns towards
                       the y axis)

        conv        conversion  factor to  enable the  printout in  various
                    practical units.  For feet,   the conversion factor is,
                    for example  0.3048 (the length  of a foot  in meters).
                    The program recognizes yards,  feet,  inches,  cm,  mm,
                    microns.   However,  any conversion  factor is accepted
                    even if not recognized.
     
        mprint -2   no printing of results.
               -1   printing final result only.
                0   print all intermediate and final results.
                n   n>0 used  with list,   there are  n intervals  in which
                    printing will occur.

        list        contains the beginning and the  end of all intervals in
                    which printing  is done.    List is a  set of  pairs of
                    numbers.  List may contain up to mxlist numbers (set at
                    40 initially)

------------------------------------------------------------------------

<span id="hardware_values_listing_of_machine"></span><span id="interactive_control_of_lattice"></span>

<span id="interactive_control_of_lattice"></span>

## <span id="interactive_control_of_lattice">INTERACTIVE CONTROL OF LATTICE</span>

<span id="interactive_control_of_lattice">This operation enables to vary
parameters of chosen elements while observing the beam at an end point.
The beam has to be defined in a previous BEAM operation and a previous
GENERATION of particles. The beam caqn be observed in a printer-plot or
by its statistical parameters. The particles are tracked individually in
each element. This operation is not fully developped and
debugged!</span>

Input format:

        INTEractive control of ..(up to 80 char)
        niopt nivar
        name keyword (repeated nivar times)

     Parameters:
        niopt       option parameter not used now .
        nivar       number of parameters to be varied (maximum 8)

At run time follow the instructions of the program. This CANNOT work if
at implementation of the program the output channel 9 has NOT been
assigned to the terminal.

------------------------------------------------------------------------

<span id="interactive_control_of_lattice"></span><span id="least_square_fit"></span>

<span id="least_square_fit"></span>

## <span id="least_square_fit">LEAST SQUARE FIT</span>

<span id="least_square_fit">This operation handles any fitting problem.
Some care must be exercised in the choice of nstep and nit. Experience
will show what choices are best suited to the problem. A safe choice is
2 2 (1 1 is faster but less accurate). If the program is very slow at
finding a solution or if overflow condition is developed in the
subroutine LMDIF, the solution sought is probably not a practical one. A
new minimizer(LMDIF) was installed in December 1984. It has a default
tolerance and default increments for the variables which seem adequate.
As a consequence the input parameters del(i) have no influence. We have
kept them to avoid changes in the input format until we are satisfied
with the new minimizer.</span>

Input format:

        LEASt square fit of .....(up to 80 char)
        nstep nit nvar ncond
        betax,alphax,etax,etapx,betay,alphay,etay,etapy
        name(i)  pkeyw(i) del(i)   for i = 1 to nvar
        nval(j)  valf(j) weight(j) for j = 1 to ncond
        nasp
        repeat the following nasp times
        name1 npas
        name(k) pkeyw(k) coef(k)  for k = 1 to npas

     Parameters:

        nstep       number of steps taken to approach final fit.

        nit         number of iterations used in final step of fit.

        nvar        number of independent variables(max:20).

        ncond       number of conditions to be met(max:20).

        betax...etapy
                    initial values needed for the function computation.When
                    betax value is entered as  zero,  then the program uses
                    the betax...etapy  values computed  in the  last matrix
                    operation preceding the present operation.

        name(i)     name of  element with  an independent  parameter to  be
                    varied.

        pkeyw(i)     variable element parameter keyword

        del(i)      this  parameter  is  not  used  in  the  new  minimizer
                    implementation,  but was  kept in the input  to avoid a
                    major change in the input format.

        nval(j):    reference number of output value to be fitted.  Numbers
                    1  to  20 are  for  the  values  of the  stable  motion
                    analysis  of the  total  matrix in  the  same order  as
                    mentioned in paragraph 2.8. of the SIMP operation.
                    Numbers 21 to  30 refer to betax alphax  etax etapx nux
                    betay alphay etay etapy nuy at  the end of the machine.
                    These  values  are  computed from  the  initial  values
                    present in the second line of the input format.
                    Numbers 31 to 40 refer to the values betax nuy computed
                    at the first fit point defined by the preceding SET Fit
                    point operation.
                    Numbers 1031  to 1040 refer  to the  difference between
                    the values betax  nuy computed at the  first and second
                    fit  point  defined  by the  preceding  SET  Fit  point
                    operation.(ie.:v2-v1)
                    Numbers 41  to 61  refer to  the beam  values sigx  ...
                    sigp  and the  rij at  the  end of  the machine.    See
                    operation BEAM for the meaning  of these parameters and
                    the order in which they appear.   These values can only
                    be fitted if a BEAM  operation defining the beam values
                    at the begining of the machine has preceded the fitting
                    operation.
                    Numbers 71 to 91 refer to the same beam values computed
                    at the first fit point defined by the operation SET Fit
                    point.
                    Numbers 1071  to 1091 refer  to the differences  of the
                    same beam values  computed at the first  and second fit
                    point    defined    by   the    operation    SET    Fit
                    point.(ie.:v2-v1)
                    Numbers 93 to  98 fit the average  chromatic errors for
                    betax, alphax,  betay,  alphay nux,  nuy as computed in
                    the detailed  chromatic analysis operation.   A  fit on
                    these  elements  can  only be  done  after  a  previous
                    detailed  chromatic analysis  operation  is done  which
                    serves  to   define  the  parameters  needed   for  the
                    computation.
                    Selected numbers 110 to 666  specify matrix elements in
                    the following fashion:
                      ij0 represents the first order matrix element R(i,j).
                      ijk  represents  the  second   order  matrix  element
                      T(i,j,k) (as in the TRANSPORT notation).

        valf(j)     value to be achieved.

        weight(j)   weight attached to the value(j) in the fit function.

        nasp        number of associated parameters.  If nasp = 0, then the
                    following data is not to be entered.

        name1       name of  the basic  parameter to  which the  associated
                    parameters are connected.    It must be present  in the
                    list of basic parameters.

        npas        number of parameters to be associated to name1 (max:6).

        name(k)     name of  one element having  a parameter  associated to
                    name1.
     
        pkeyw(k)     keyword of  the parameter of name(k)   associated with
                    name1.

        coef(k)     coefficient  with which  the  BASE  parameter (that  of
                    name1)  is to be multiplied to  obtain the value of the
                    parameter of name(k).

------------------------------------------------------------------------

<span id="least_square_fit"></span>
<span id="line_geomtric_abberations"></span>

## <span id="line_geomtric_abberations">LINE GEOMETRIC ABERRATIONS (ONE TURN COMPUTATION)</span>

<span id="line_geomtric_abberations">This operation traces npart
particles, placed on ellipses with nominal emittances epsx(i), epsy(i)
for one turn. It then fits an ellipse to the output points obtained.
From this fitted ellipse it determines the average values for betax,
alphax, betay, alphay, nux, nuy, epsx, and epsy.</span>

Input format:

        LINEgeometric aberrations....(up to 80 characters)
        betax,alphax,betay,alphay
        xco,xpco,yco,ypco,ener
        ncase,npart,ncoup
        nplot,nprint,mlocat,[list]
        epsx   , epsy
            1        1
         ....................
         epsx      ,epsy
             ncase      ncase

     Parameters:

        betax,alphax,betay,alphay
                    input values of the twiss parameters at the entrance of
                    the line.

        xco,xpco,yco,ypco,ener
                    coordinates and  the momentum of the  trajectory around
                    which the aberrations are to be computed.

        ncase       number of cases analyzed(maximum 10).

        npart       number of particles to be traced(maximum 300).

        ncoup       not used presently, but a value must be inserted.

        nplot    1  plot the resulting particles at the end of the job.  It
                    computes its own plotting windows.
                -1  no plotting.

        nprint  -2  no printing.
                -1  printing at end of the line only.
                 0  printing after every element

        mlocat      number of intervals in which printing is to occur; used
                    in conjunction with list.  This is not yet implemented.

        list        intervals in which printing  occurs.   List may contain
                    up to mxlist numbers (set at 40 initially)

        epsx  , epsy    ncase values for the chosen nominal
            i       i   emittances in x and y  using the unit
                           mm-mrad (E-06 m-rad)

------------------------------------------------------------------------

<span id="line_geomtric_abberations"></span>
<span id="machine_and_beam_parameters_computations"></span>

## <span id="machine_and_beam_parameters_computations">MACHINE AND BEAM PARAMETERS COMPUTATIONS</span>

<span id="machine_and_beam_parameters_computations">Computes beta,
alpha, eta, etap, nu values at selected points around the machine. If
requested beam parameters are computed. In some cases, the optimum
coupling values may be meaningless(if coupling is \> 1).</span>

Input Format:

        MACHine and beam parameters....(up to 80 char)
        E1 E2 dE NLUM DNU NINT NBUNCH
        betax alphax etax etapx
        betay alphay etay etapy MPRINT (LIST)

**Note**: if E1 is zero then nlum is assumed 0 and the input twiss
parameters values are those obtained in a previous matrix analysis. If
E1 is non zero but betax is zero, the first line of parameters must be
given and the initial twiss parameters values will be those of the
preceding matrix analysis.

     Parameters:

        E1          start   momentum   for   beam   data   and   luminosity
                    computations.

        E2          end momentum

        dE          momentum step

        NLUM    0   no beam size related computations are done.
                1   synchrotron integrals and basic  beam size computations
                    are done.
                2   Full luminosity computations are made.

        DNU         dnu value used for optimum luminosity computation.
     
        NINT        number of interaction regions

        NBUNCH      number of bunches

        betax...etapy
                    function values at starting point of lattice.

        mprint -2   no printing of results
               -1   print final result only.
                0   print all intermediary and final results.
                n   n>0 is used with list.   There are n intervals in which
                    printing will occur.

        list        beginning and end  positions of each interval  in which
                    printing is done.   List is a  set of pairs of numbers.
                    List  may  contain up  to  mxlist  numbers (set  at  40
                    initially)

------------------------------------------------------------------------

<span id="machine_and_beam_parameters_computations"></span>
<span id="matrix_computation"></span>

## <span id="matrix_computation">MATRIX COMPUTATION</span>

<span id="matrix_computation">Computes matrices and performs movement
analysis on matrix obtained at end of lattice. Input Format:</span>

        MATRix computations...........(up to 80 char)
        NORDER  MPRINT  [LIST]

     Parameters:

        NORDER  1   first order matrix only is printed.
                2   second order terms are also printed.
               <0   computation is done to order abs(NORDER).   MPRINT must
                    be >0 and  the program will print matrices  of the beam
                    line situated  between and including the  element pairs
                    defined in LIST.
                    When norder  is -1 or  -2 the  format of the  output is
                    identical to the input format of the Gxxxxxxx element.
                    When norder is -11 or -12  th computation is to order 1
                    and 2 and the format of the output the standard program
                    output for matrices

        MPRINT -2   no printing of matrix.
               -1   print matrix at end of machine only.
                0   print all  intermediary  matrices  plus  final matrix.
                n   where n>0,  used with LIST  and indicates the number of
                    intervals in which printing is to occur.

        LIST        set of  pairs of numbers  which indicate  the beginning
                    and  end  position  (in  the  order  list  of   machine
                    elements) of each of mlocat intervals in which printing
                    takes place.    List may contain  up to  mxlist numbers
                    (set at 40 initially)

------------------------------------------------------------------------

<span id="matrix_computation"></span>
<span id="modification_of_element_data"></span>

## <span id="modification_of_element_data">MODIFICATION OF ELEMENT DATA</span>

<span id="modification_of_element_data">Enables user to change input
parameters between successive operations. It is particularly useful in
simulations of injection and extraction processes in conjunction with
the kick elements and the TRACking operation.</span>

Input Format:

        MODIfication of input parameters...(up to 80 char)
        n
        name   pkeyw      value
        name   pkeyw      value
                   . . .
        name   pkeyw      value

     Parameters:

        n           number of  varies to be made  (= number of  'name pkeyw
                    value' entries which follow).

        name        name of the machine element which is to  be varied.

        pkeyw       keyword of the parameter in  the given element which is
                    to be changed.

        value       value the parameter is to be changed to.

------------------------------------------------------------------------

<span id="modification_of_element_data"></span>
<span id="movement_analysis"></span>

## <span id="movement_analysis">MOVEMENT ANALYSIS</span>

<span id="movement_analysis">This operation finds closed orbits and
analyses both stable and unstable motions for up to 15 different
momenta. **NOTE**: no kicks simulating synchrotron oscillation
(parameter T \< 0 ) may exist in the lattice. Results are meaningless in
the presence of such kicks.</span>

Input Format:

     
        MOVEment analysis ..... (up to 80 char)
        nprint nturn nanal nit nener ncoef dist
        x x' y y' l  e .....e
                      1      nener
        naplt delmin delmax dnumin dnumax dbmin dbmax ncol nline

      Parameters:

        nprint      print action for the closed orbit information
                0   action after every element
                n   action after every n turn

        nturn       number of turns over which  analysis is performed.   It
                    enables user to study higher order resonances.

        nanal   0   no stability analysis is done, only the closed orbit is
                    computed.
                1   stability  analysis  is  performed   (both  stable  and
                    unstable).
                2   gives information about an order two resonance.  (NTURN
                    must then be equal to 2.)
                3   gives  information  about  an  order  three  resonance.
                    (NTURN must then be equal to 3.)
                    NOTE:  in both cases where NANAL is equal to 2 or 3 the
                    resonance motion analyzed  is supposed to occur  in the
                    horizontal phase  plane.  If  the user  wants to  study
                    resonance in the vertical plane,  the machine should be
                    set up so that its planes are exchanged.
                     In  the versions  subsequent  to  April 1  1988,   the
                    coordinates  of the  particles  close  to the  unstable
                    fixed  point  of  the first  momentum  are  stored  for
                    subsequent use in a tracking  operation.  See the demo3
                    input file for its use

        nit         number of  iterations used.   ABS(nit)   iterations are
                    performed.   If nit is negative only the results of the
                    last iteration are printed.

        nener       number of momenta for which  the analysis is performed.
                    (max 15)

        ncoef       number  of  coefficients  to  be  used  in  the  Taylor
                    expansion of the parameters nu,   beta,  eta,  and etap
                    versus  momentum.    The  reference   momentum  in  the
                    expansion is fixed at 0.01(1%).  (max 6)  The reference
                    momentum  can be  changed  by  the CONStant  definition
                    operation.
     
        dist        indicates  the 'distance'  (in phase  space)  from  the
                    estimated position  of the  closed orbit  at which  the
                    particles  needed  for the  computation  are  initially
                    generated.   A safe choice is 0.001 or some lower value
                    depending on  the size of  the phase space  occupied by
                    the beam.

        x,x',y,y',l estimate of the coordinates of the closed orbit.

        e ....e     Momenta(dp/p) for which the analysis is

         1     nener   performed.

        naplt   0   no plot of the Taylor expansion is required
                1   a plot is required

        delmin delmax
                    min max of dp/p for the plot.

        dnumin dnumax
                    min  max for  dnu  (the first  momentum  serves as  the
                    reference to compute the tune difference dnu).

        dbmin dbmax min max for relative difference in betas.

        ncol nline  number of columns and lines desired for plot.

------------------------------------------------------------------------

<span id="movement_analysis"></span><span id="output_control"></span>

<span id="output_control"></span>

## <span id="output_control">OUTPUT CONTROL</span>

<span id="output_control">This operation provides control of the output
printout It affects only the dimat part of the output.</span>

Input format:

        OUTPut control
        nopt

     Parameters

        nopt  0  all output is supressed (except error messages)
              1  The main results of the computation only are printed
              2  The main results and the input data are printed
              3  All output is printed
              4  Used  for short  printing of  tracking  results when  such
                    printing can be used for input to plotting programs.
              14 Same as 4  .  The fifth coordinate will then  be the phase
                    relative to an RF cavity instead of the path length.

     Notes :        not all output has been affected  by this option in the
                    present version of the program In the operation section
                    of  the input  data,  this  option  should only  appear
                    between two  operation arrays and  not inside  one such
                    array.   This  operation cannot appear in  the Standard
                    format  input section.    In this  section the  command
                    NOECHO can be used to  suppress printout and the NOECHO
                    can be reversed by use of the command ECHO.

------------------------------------------------------------------------

<span id="output_control"></span>
<span id="particle_distribution_analysis"></span>

## <span id="particle_distribution_analysis">PARTICLE DISTRIBUTION ANALYSIS</span>

<span id="particle_distribution_analysis">This operation is destined to
provide some analysis of particle distribution.</span>

Input format:

        PARTicle distribution analysis
        nopt

     Parameters

        nopt must be  equal  to  the  parameter   chosen  in  the  particle
                    generation  When equal  to 1  the values  for the  beam
                    sizes  are  independent  of the  choice  of  the  scale
                    parameter of the GENEration operation. This facilitates
                    comparison between  similar beams with  different scale
                    factors(useful in non linearity studies)

------------------------------------------------------------------------

<span id="particle_distribution_analysis"></span>
<span id="print_selection"></span>

## <span id="print_selection">PRINT SELECTION</span>

<span id="print_selection">This operation allows to determine points or
intervals at which results should be printed. Whenever, in some
operation,the printing option is set to -2,-1 or 0 it does supercede the
print selection of the present operation. If a print selection has been
defined, the print option within any subsequent operation should be set
positive.</span>

Input format:

            PRINt selection
            keyword
            name(i)    as many as needed
            99
            end,

           The different keywords are : interval, name, type

           Interval :  allows to define up to MXLIST intervals (default 40)
                    The intervals are defined by pairs of names of elements
                    present in the currently used beamline.  The names must
                    be in ascending  order of position and  must be unique.
                    Preferably one shoulduse markers.   99 is the flag that
                    terminates the  sequence of names.   99 is  followed by
                    another keyword or by end,.

            name : followed by  names of elements  at which printing  is to
                    occur. The maximum number is 10.  The names may contain
                    the wild  character * .   AB* means all  names starting
                    with ab will produce printing.

            type : the types are :   drift,  bend,  quadrupole,  multipole,
                    gkick,  collimator,   rfcavity,  sextupole,   solenoid,
                    monitor,  quadsext,  matrix,  mtwiss,  arbitrary.   Two
                    types may be defined.

------------------------------------------------------------------------

<span id="program_generation"></span>

## PROGRAM GENERATION

**NOTE** TEMPORARILY THIS OPERATION IS NOT AVAILABLE This operation is
destined to produce a fortran subroutine that can be used to trace
particles along a beamline with the minimum computing overhead. The
machine is fully exploded and no array addressing is used. In this sense
it is useful for parallel processing of the particle tracking.The
misalignments and errors are implemented.

Input format: **PROG**ram generation nopt nint a(1) b(1) ..... a(nint)
b(nint) Parameters nopt option number to choose a statement separator 0
no separator character is used 1 ; is the statement separator character
(useful for C language) 2 : is the statement separator character. nint
the number of intervals in which the generation is to occur. a(i),b(i)
the begining and end of the ith interval in which the program simulating
tracking of a particle in the segment a(i),b(i) is done

------------------------------------------------------------------------

<span id="program_generation"></span>
<span id="rmatrix_computation"></span>

## <span id="rmatrix_computation">RMATRIX COMPUTATION (6X6)</span>

<span id="rmatrix_computation">Computes in chosen intervals the 6X6
transfer matrix of the beamline comprised in these intervals. The
computation is done by the tracking of seven particles chosen around a
given initial set of coordinates.</span>

This enables to determine the first order behaviour of beamlines
affected by errors and misalignements. The program also provides the
entrance and exit orbit displacements. They are needed to correctly use
the matrix.

Input format:

        RMATrix.......................(up to 80 characters)
        x0 xp0 y0 yp0 l0 delta0
        dx dxp dy dyp dl ddelta
        norder mprint

     Parameters:

         x0 xp0 y0 yp0 l0  delta0  initial  coordinates of  reference orbit
                    When  delta0   =  1  then   the  coordinates   are  the
                    coordinates of the closed orbit  computed in a previous
                    movement  analysis.  The  values  corresponding to  the
                    first energy are used.

         dx dxp dy dyp  dl ddelta   increments  used  to generate  the  six
                    particles surrounding the reference orbit.

         norder        order of the computation: 1 or 11 . The order of the
                    computation is 1. when norder = 11 the output is in the
                    standard input format.

         mprint     number of intervals wanted

         nlist      mprint  pairs of  numbers  defining  the intervals  for
                    which the matrix will be computed.

------------------------------------------------------------------------

<span id="rmatrix_computation"></span>
<span id="sho_values_of_constants"></span>

## <span id="sho_values_of_constants">SHO VALUES OF CONSTANTS</span>

<span id="sho_values_of_constants">This operation displays the values of
the basic constants used in the program. Input format :</span>

        SHO Values of the basic constants

no parameters are used for this operation.

------------------------------------------------------------------------

<span id="sho_values_of_constants"></span>
<span id="simple_fitting"></span>

## <span id="simple_fitting">SIMPLE FITTING</span>

<span id="simple_fitting">This operation is used for easy fitting
(tunes, chromaticity). It uses Newton's method involving an equal number
of conditions and variables.</span>

Input format:

        SIMPle fitting ........(up to 80 char)
        nstep nit nvar
        name  pkeyw  del     (i = 1 to nvar)
            i      i    i
        nval  valf          (i = 1 to nvar)
            i     i
        nasp

        repeat the following nasp times
        name pkeyw npas

        name(k) pkeyw(k) mult(k) add(k)  k = 1 to npas Parameters:

        nstep       number of steps to reach the final stage.

        nit         number  of iterations  performed in  the  last step  in
                    order to refine the variable values.

        nvar        number of variables and conditions (max:10).

        name        name of element containing a variable.

        pkeyw       keyword of the parameter to be varied in the element.

        del         increment by which the variable is to be varied.   This
                    number is divided  by 5 in every iteration  of the last
                    step.

        nval        order number  of value to  be achieved.   The  order is
                    given by  the following  list:   compf  nux etax  etapx
                    alphax   betax   dmux/ddelta    chromx   dalphax/ddelta
                    dbetax/ddelta   muy  nuy   etay   etapy  alphay   betay
                    dmuy/ddelta chromy dalphay/ddelta dbetay/ddelta,  where
                    compf stands  for the compaction  factor in  x.   These
                    values are those computed in the stable motion analysis
                    of the  matrix of  the complete  machine.Note that  the
                    momentum dependence of eta cannot be fitted

        valf        the values to be achieved in the final step.

        npas        total  number of  parameters to  be  associated to  the
                    parameter pkeyw of name1.

        name(k)     name  of  the  element  which has  a  parameter  to  be
                    associated with name 1.

        pkeyw(k)     parameter keyword to be associated.

        mult(k),add(k)
                    multiplicative and additive constants  which define the
                    value  of the  associated  parameter  according to  the
                    following formula
                       parvalue(k) = mult(k)*parval + add(k)
                    where parval is the value of  the parameter used in the
                    element name1 and to which pkeyw(k) is associated.

------------------------------------------------------------------------

<span id="simple_fitting"></span>
<span id="seismic_perturbation_simulation"></span>

## <span id="seismic_perturbation_simulation">SEISMIC PERTURBATION SIMULATION</span>

<span id="seismic_perturbation_simulation">Sets transverse
misalignements according to sinewaves of some chosen frequency and
amplitude as a function of the longitudinal coordinate. The vertical
oscillation may be different from the horizontal oscillation.</span>

This operation only affects the tracking of particles and all operations
that use tracking.

Input format:

        SEISmic simulation............(up to 80 characters)
        xlambs axseis phixs
        ylambs ayseis phiys
        beginname endname

     Parameters:

        xlambs,ylambs Wave length(in m) for x and y waves

        axseis,ayseis Amplitudes of the x and y waves

        phixs,phiys x and y phase shift of each wave.

        beginname,endname  name of elements where the  wave is to start and
                    where  it  is  to stop.   Use  unique  names,   markers
                    preferably.

------------------------------------------------------------------------

<span id="seismic_perturbation_simulation"></span><span id="set_fit_point"></span>

<span id="set_fit_point"></span>

## <span id="set_fit_point">SET FIT POINT</span>

<span id="set_fit_point">Sets an intermediate fit point to be used with
the least square fit operation.</span>

Input format:

        SET Fit point.................(up to 80 characters)
        n Position1 (Position2)

     Parameters:

        n            Number of fit points defined (maximum 2)

        Positioni    Name (must be unique in  machine list)  of the element
                    after  which the  fitted  values  are applied.   It  is
                    recommended to use a marker for this purpose.

------------------------------------------------------------------------

<span id="set_fit_point"></span><span id="set_limits_to_variables"></span>

<span id="set_limits_to_variables"></span>

## <span id="set_limits_to_variables">SET LIMITS TO VARIABLES</span>

<span id="set_limits_to_variables">Sets boundaries on the variables used
in the least square fitting operation. This is done via a supplementary
constraint that uses an internally defined penalty function. When
boundaries are in use the achieved value for the fit function will
depend strongly on the distance between the "ideal" minimum is from the
boundary values when that minimum is outside the boundaries. Choice of
weights will change greatly the final values achieved.</span>

Input format:

     
        SET Limits on variables ......(up to 80 characters)
        name keyword upper lower weight distance
        ........................................
        name keyword upper lower weight distance
        99,

     Parameters:

        Name         Name of the  element for which some  parameter will be
                    affected by boundaries.

        Keyword      Keyword  of  the  parameter  to  be  affected  by  the
                    boundaries.

        Upper Lower  The   upper  and   lower   boundaries  affecting   the
                    parameter.

        Weight       The weight that is applied to the boundary constraint.

        Distance     A  distance parameter  that  serves  to indicate  some
                    flexibility in the boundary  constraint.  That distance
                    is  progressively   reduced  as  the   iterations  step
                    increases.

------------------------------------------------------------------------

<span id="set_limits_to_variables"></span><span id="set_symplectic_option_on"></span>

<span id="set_symplectic_option_on"></span>

## <span id="set_symplectic_option_on">SET SYMPLECTIC OPTION ON</span>

<span id="set_symplectic_option_on">Sets the symplectic option on. As
soon as this operation is executed all the matrices are transformed to
the six dimensional space defined by the canonical variables
x,px,y,py,-tau=-t\*c=-al,DE/E. Please note that,in the present
implementation the approximation v/c=1 was made. All the movement
analyses performed in the program relate to the matrix and so the values
will change when this option is on. Please note that this operation
changes the sign of the fifth parameter. This may need to be taken into
account in the definition of the lag parameter of cavities!!!.</span>

**NOTE**: In the present version of the program , this option cannot be
followed by any fitting which changes matrices. Any fitting not changing
matrices is allowed (eg: in alignment fitting when steering only is
involved)

Input format:

        SET Symplectic option on......(up to 80 characters)
        Option Energy

     Parameters:

        Option       Determines the mode of tracking. This affects only the
                    operations based  on tracking  and not  those based  on
                    matrix   analysis  (MATRIX,    BEAM  MATRIX,    MACHINE
                    FUNCTIONS)
                0  non symplectic  ray trace  is done  using the  canonical
                    matrices.
                1  Fast version  of ray  trace is  done with  the variables
                    x,x',y,y',al,delta and using the canonical matrices.
                2  Fast version  of ray  trace is  done with  the variables
                    x,px,y,py,-tau,DE/E and using the canonical matrices.
                3  Slow version  of ray  trace is  done with  the variables
                    x,x',y,y',al,delta and using the canonical matrices.
                4  Slow version  of ray  trace is  done with  the variables
                    x,px,y,py,-tau,DE/E and using the canonical matrices.

     Note: Options 3 and  4 are not for  the general user.  They  have been
                    used and maintained for debugging purposes only.

        Energy       Energy of nominal particle (in GeV)

------------------------------------------------------------------------

<span id="set_symplectic_option_on"></span>
<span id="space_charge_computation"></span>

## <span id="space_charge_computation">SPACE CHARGE COMPUTATION</span>

<span id="space_charge_computation">This operation computes the linear
effect of space charge on the motion of individual particles. It only
affects operations that use tracking. This operation MUST be used in
conjunction with a gkick simulating synchrotron oscillation or with an
rfcavity. Particles at the centre of the bunch are affected by the
maximum tune shift and particles at the edge see a zero tune shift. The
simulation is done by changing the effective strength of each quadrupole
according to the relative particle momentum position with respect to the
momentum spread assumed to be present in the beam. This operation is
introduced on an experimental basis only. We recommend caution in the
interpretation of the results.</span>

Input format:

        SPACe charge computation...(up to 80 char)
        option dpmax dkmax,

     Parameters:

        option   0 sets the computation off, 1 sets it on.

        dpmax    The maximum dp/p present in the beam

        dkmax    The maximum  relative change all quadrupole  settings that
                    will produce  the required  maximum space  charged tune
                    shifts as determined by the  theory of linear tuneshift
                    produced by space  charge.  The link between  dkmax and
                    the maximum space charge tune  shift is generally given
                    by the natural chromaticity of the machine.

------------------------------------------------------------------------

<span id="space_charge_computation"></span>
<span id="tracking_of_particles"></span>

## <span id="tracking_of_particles">TRACKING OF PARTICLES</span>

<span id="tracking_of_particles">Tracks up to 1000 particles around the
machine.The initial values of the coordinates of the particles are lost
in the process of tracking.They are replaced by the final values of the
coordinates.</span>

Input format:

        TRACking of particles .....(up to 80 char)
        NPLOT NPRINT NPART NTURN
           particle data ( x x' y y' l dp - for all particles)
        MLOCAT  LIST  NGRAPH XMIN XMAX XPMIN XPMAX YMIN YMAX
        YPMIN YPMAX NCOL NLINE (ALMIN ALMAX DELMIN DELMAX)

     Parameters:

        NPLOT   0   plot action after every element.
               -1   no plot action.
                n   action occurs after  n turns (used in  conjunction with
                    MLOCAT and LIST)  at MLOCAT locations specified by LIST
                    elements.

        NPRINT  0   print action after every element.
               -1   printing at end only.
               -2   no printing occurs.
                n   action occurs after  n turns (used in  conjunction with
                    MLOCAT and LIST)  at MLOCAT locations specified by LIST
                    elements.
                     NOTE: when nplot=-1 and nprint=-2 then mlocat and list
                    do not appear.MLOCAT and LIST are the same for plot and
                    print.

        NPART       number of particles traced.
               <0   abs(npart) particles  are  added  to  particles already
                    present from previous operation.
                0   existing particles kept - none added.
               >0   previously used particles deleted.  NPART new particles
                    introduced.

        NTURN       number of turns to be traced.
     
        x x' y y' l dp
                    particle data for NPART particles.

        MLOCAT      indicates the number of intervals  in which printing is
                    to occur.    If MLOCAT  is equal  to 0,   then printing
                    occurs at end of lattice only.    When MLOCAT is 0,  no
                    number is in list.

        LIST        set of pairs  of numbers,  each of  which indicates the
                    begining  and  end  position  (in  the  order  list  of
                    machine elements)  of each of MLOCAT intervals in which
                    printing takes place.    List may contain up  to mxlist
                    numbers (set at 40 initially)

        NGRAPH  1   plot x,x' plane
                2   plot y,y' plane
                3   plot x,y plane
                4   plot all planes
                11,12,13,14
                    as above but the graphs are accumulated and the plot is
                    printed at the end.
                15,16
                    accumulates the al,del or the Phi,del plots where al is
                    the pathlength coordinate of the particles,  Phi is the
                    phaseshift  with  respect  to   the  frequency  of  the
                    cavities (they  must be  present for  this graph  to be
                    meaningful , the cavities need not be in phase with the
                    total  length   of  the   machine).del  is   the  sixth
                    coordinate of the particles Note that 15 will present a
                    correct plot  of the  longitudinal phase-space  only if
                    the  frequency of  the  cavity and  the  length of  the
                    machine match perfectly (8 digits usually!!). Using the
                    value  16 garantees  a  plot which  uses  the RF  phase
                    instead  of  path  length  differences  and  is  always
                    readable.
                17
                    is equivalent to 14 as regards the xx',yy' and xy plots
                    and  at  the same  time  will  produce  an E  phi  plot
                    identical to  that of produced  by the ngraph  value of
                    16.

        XMIN,XMAX,XPMIN,XPMAX,YMIN,YMAX,YPMIN,YPMAX
                    limits for the plotting windows.

        ALMIN,ALMAX,DELMIN,DELMAX
                    limits for the plotting windows for the cases NGRAPH=15
                    or  16 The  are not  present  for the  other values  of
                    NGRAPH For NGRAPH=16 AL is to be interpreted as PHI.

        NCOL,NLINE  number of  columns and  number of lines  to be  used in
                    plot matrix.

------------------------------------------------------------------------

<span id="tracking_of_particles"></span><span id="operations_used_in_conjuction"></span>

<span id="alignment_fitting"></span>

## <span id="alignment_fitting">OPERATIONS USED IN CONJUNCTION WITH MISALIGNMENTS AND ERRORS</span>

<span id="alignment_fitting">General note of caution : random generators
produce different sequences on different computers even when using the
same initial seed. So results provided in the demos using such random
generation may vary in the detail though the trends will be
similar.</span>

## ALIGNMENT FITTING

This operations allows user to fit values read in monitors (see their
definition in the machine list). Any parameter can be used as variable.
Successive use of this operation can simulate progressive alignment
correction of a beamline. A new minimizer is installed since December 1
1984. It has a default tolerance and default increments for the
variables which seem adequate. As a consequence the input parameters
del(i) have no influence. We have kept them to avoid changes in the
input format until we are satisfied with the new minimizer.

Input format:

        ALIGnment fitting .....(maximum 80 characters)
        nstep nit nvar ncond nfit nopter
        betax alphax etax etapx
        betay alphay etay etapy
        x  x'  y  y'
         0  0   0  0
        dx dx' dy dy'
        nener ener ...  ener
                  1         nener
        Origin
        name  keywd  del
            1      1    1
        ................
        name     keywd     del
            npar      npar    npar

      When nfit equals 1 or 2 the following group applies
        CORR
        name pos opt param del
            1   1   1     1   1
        ..........................
        name    pos    opt     param    del
            ncor   ncor   ncor      ncor   ncor

     NOTE: ncor+npar=nval
        mon  pos val# value  weight  error
           1    1    1     1       1      1
        ..........................
        mon     pos     val#     value     weight     error
           ncond   ncond    ncond     ncond      ncond     ncond

      End of the group for nfit 1 or 2
      If nfit equals 3 the following group applies :
        CORR
        mcorr
        name(i) opt(i) param(i)  for i=1 to mcorr
        nmon nskip
        name(i) val#(i) value(i) weight(i) error(i) for i=1 to nmon 
      end of group for nfit 3 
        nasp
        repeat the following nasp times
        name keywd npas
        name(k) keywd(k) mult(k) add(k)  k = 1 to npas

     Parameters:

        nstep       number of steps taken to approach final fit.

        nit         number of iterations used in final step.   During these
                    iterations  the stepsizes  del  of  the parameters  are
                    reduced by a constant factor(5).

        nvar        number of variables used (maximum 12).

        ncond       number of conditions fitted.

        nfit        selects the fitting procedure.
                 1  Newton's method is used.  In this case ncond = nvar.
                 2  A least square fit is used.

        nopter      error option parameter for the reading of the monitors,
                    this is a noise error of the reading.
                 0 the monitors have no errors
                 1 the  monitor  error is  the  value  given in  the  error
                    parameter of the monitor(see below) multiplied randomly
                    by + and - signs.
                 2 The monitor error is a  uniform random distribution with
                    a sigma equal to the error value.
                 3 The monitor error  is a gaussian distribution  cutoff at
                    two sigmas
                 4 The monitor error  is a gaussian distribution  cutoff at
                    six sigmas
                11,12,13,14 the random error is the  same as for 1,2,3 or 4
                    with a fast  random generation of the  random sequence.
                    This  random could,   in some  cases,   be affected  by
                    unwanted correlations.   In case  of doubt,   check the
                    STATISTICAL validity of  your results with a  family of
                    runs using the options 1,2,3 or 4.

The initial seed used is the same as that defined by the operation SEED.
The generation of the random errors for the monitors is, INDEPENDENT of
that of the misalignments and of the field errors.

        betax,alphax,betay,alphay
                    input parameters  used in the  computing the  beam line
                    function values

        x ,x',y, y' initial values of nominal orbit.
         0  0  0  0

        dx dx' dy dy'
                    increments used in  the computation of the cx  sx cy sy
                    functions  needed  to generate  the  transfer  matrices
                    around the nominal orbit

        nener       number of momenta traced (maximum 3).

        ener        values of the momenta (p-p0)/p0.

        origin   position used as current origin to position the correctors
                    used later.  This position is  be specified by the name
                    of an element.

        name keywd
            i     i  names of the elements having  parameters to be varied.
                    npar<=nvar such elements can be used

        del         not used in the present version, but must be present in
                    the input.

        CORR     flag to signal the correctors are going to be used

        mcor         for fit 3 : number of corrector names. The
                      program  picks the  first  ncor available  correctors
                    whose name are  any of name(i).   Remember  that ncor =
                    nvar-npar

        name(i)      name of corrector
        pos       relative position (origin + pos  is the absolute
          i      position of the corrector)     i

        opt       option defining the type of corrector(see SETCorrector
          i      operation

        param     parameter number of parameter to be varied
            i
     
       del       increment  used  in  the  fitting   routine  to  vary  the
                     parameter
          i

        mon(i)      Monitor name as present in machine list

        nmon        for nfit 3 :  names  of distinct monitors.  The program
                    picks  ncond monitors  whose name  fits name(i)   AFTER
                    SKIPPING nskip monitors!

        pos(i)      Relative position  of the monitor  with respect  to the
                    origin point.

        Val#(i)     value number
                    =(iener-1)*4+1 x value as read by monitor
                    =(iener-1)*4+2 y value as read by monitor
                    =(iener-1)*4+3 sigx value as read by monitor
                    =(iener-1)*4+4 sigy value as read by monitor

        value(i)    values read  are those corresponding to  momentum iener
                    (1 to 3 maximum).

        weight(i)   used in  conjuction with  the least  square fit.   This
                    parameter  enables  the  user to  put  more  weight  on
                    certain values to be fitted.   The higher the weight(i)
                    the stronger the constraint to fit the value(i).

        error(i)    used  in conjunction  with the  parameter nopter.    If
                    nopter is zero no error affects the monitors. If nopter
                    is  1 the  monitor  mon(i)  is  affected  by the  error
                    error(i).

        nasp:       number of associated parameters

        name1       Name  of element  to which  some parameters  are to  be
                    associated.

        keywd       parameter  keyword  of  element  name1  to  which  some
                    parameters are to be associated.

        npas        total  number of  parameters to  be  associated to  the
                    parameter par# of name1.

        name(k)     name  of  the  element  which has  a  parameter  to  be
                    associated with name 1.

        par#(k)     keyword of parameter  to be associated.

        mult(k),add(k)
                    multiplicative and additive constants  which define the
                    value  of the  associated  parameter  according to  the
                    following formula
                       parvalue(k) = mult(k)*parval + add(k)
                    where parval is the value of  the parameter used in the
                    element name1 and to which par#(k) is associated.

------------------------------------------------------------------------

<span id="alignment_fitting"></span>
<span id="baseline_definition"></span>

## <span id="baseline_definition">BASELINE DEFINITION</span>

<span id="baseline_definition">This operation defines a baseline
resulting from surveying errors. The baseline must be considered like a
new reference orbit. It is obtained by two successive operations. In the
first a few points on the original reference orbit are chosen as main
surveying points. In tunnel construction they could be associated with
the surveyor's penetration points. They are accompanied by random x,y,z
coordinate errors (usually rather big : say 5 to 10 mm) The second
operation defines between the preceding basepoints intermediate points
which are obtained by successive aiming from the current point to the
next basepoint. This aiming is accompanied by a systematic aiming error
(varying from segment to segment) to which is added a random aim error
(usually smaller than the systematic error) The origin of the systematic
error can be due to the instruments used but also due to ambient
conditions under which the surveying is performed. This operation MUST
BE PRECEDED by a SEED operation. This operation is still being tested
and developped. Use at OWN RISK.</span>

Input format:

        BASEline definition
        npen Kxxxxxxx Kxxxxxxx .... Kxxxxxxx
        nsub sigma
        Kyyy Kyyy ..... Kyyy
        sigma1 sigma2

     Parameters

        npen       Number of penetration points

        Kxxxxxxx   a set of npen Elements of  the KICK type.  There MUST be
                    one such  kick at the beginning  and at the end  of the
                    lattice.

        nsub       Number of subdivision points.

        sigma      the displacement sigma  to be used in  the generation of
                    the coordinate  displacements of  the npen  penetration
                    points.

        Kyyy       nsub  KICK elements  defining  the intermediate  points.
                    They may not coincide with any of the basepoints.

        sigma1     angular sigma (in radians) of the systematic aim error

        sigma2     angular sigma (in radians) of the random aim error

------------------------------------------------------------------------

<span id="baseline_definition"></span>
<span id="block_misalignment"></span>

## <span id="block_misalignment">BLOCK MISALIGNMENT</span>

<span id="block_misalignment">This operation sets up misalignment
condition for subsets of a beamline. The whole subset is treated as if
it was one element. However, any misalignment defined in the
misalignement data definition will be superimposed. The block
misalignement is implemented via GKICK elements and uses a sequence of
random numbers that is distinct from the other random numbers used in
the program.</span>

Input format:

        BLOCk misalignement...........(maximum 80 characters)
        Name1 name2 dx dy dz dzr ddel
        .....................
        Name1 name2 dx dy dz dzr ddel
        99,

     Parameters

       name1 name2  name of two gkick elements  whose names uniquely define
                    the beamline interval to be misaligned as a block.

       dx dy dz dzr ddel   one sigma values of the random generation of the
                    x,y z offsets of the  roll around the longitudinal axis
                    and the relative field offset.
                     NOTE : only 10 distinct intervals are allowed

------------------------------------------------------------------------

<span id="block_misalignment"></span>
<span id="corrector_data_definition"></span>

## <span id="corrector_data_definition">CORRECTOR DATA DEFINITION</span>

<span id="corrector_data_definition">This operation determines which
elements in the machine are correctors By corrector we mean one element
of a family(with the same name) whose position may be changed and/or
whose setting may be changed to achieve corrections of closed orbits
and/or beam size at the monitor locations. At the moment only dipoles
with small bend angles can be used as correctors.</span>

Input format:

           CORRector data definition ....(maximum 80 characters)
           Name i1 f1.....in fn,
           .....................
           Name i1 f1 ....in fn,
           99,

Note the comma ending each line defining a corrector i1 f1 and in fn are
pairs of numbers defining the intervals in which the elements 'name' are
to serve as correctors. MAXCOR(600) distinct elements can be used as
correctors.

------------------------------------------------------------------------

<span id="corrector_data_definition"></span>
<span id="errors_data_definition"></span>

## <span id="errors_data_definition">ERRORS DATA DEFINITION</span>

<span id="errors_data_definition">This operation defines the errors that
can affect certain parameters of elements.</span>

Input format:

        ERROrs data definition ....(maximum 80 Characters)
        Name Parameter value .... parameter value;
         ...............
        Name Parameter value .... parameter value;
        99,

Note the semicolon ending each line defining errors for one element and
note the line containing 99, to end the input. Parameter and value are
the parameter keyword of the element called name that is affected by the
error. Value is the value of the error.

------------------------------------------------------------------------

<span id="errors_data_definition"></span><span id="misalignment_data_definition"></span>

<span id="misalignment_data_definition"></span>

## <span id="misalignment_data_definition">MISALIGNMENT DATA DEFINITION</span>

<span id="misalignment_data_definition">This operation defines the
misalignments of different elements of the lattice. Up to 50 distinct
elements can be misaligned.</span>

Input format:

        MISAlignment data definition....(maximum 80 characters)
        Name dm1 dm2 dm3 dm4 dz dzr ddel option
        .............
        Name dm1 dm2 dm3 dm4 dz dzr ddel option
        99,

Note the comma ending each line defining the misalignments The list is
terminated with 99, as in the first element list.

     Parameters:

For all values of the option parameter, the parameters dz and dzr are
the values of the longitudinal displacement and the rotation angle (in
radians!!) around the longitudinal axis. The values dm1,dm2,dm3 and dm4
are assumed to be small (either in displaments or angles). The program
uses approximate formulae to set up the misaligned element.

The parameter option can take the three values 1,2 or 3 which determines
the nature of the misalignment

          option = 1   The element is misaligned around  the tangent to the
                       central trajectory at the  entrance of the elements.
                       In this case the parameters  dm1,dm2,dm3 and dm4 are
                       respectively dx,dxr,dy,dyr  where dx and dy  are the
                       displacements along  the axes x  and y and  dxr ,dyr
                       are rotation angles (in radians)   around the axes x
                       and y respectively.

          option = 2   The element  is misaligned around the  chord defined
                       by the two extreme points of the central trajectory.
                       In this case the parameters  dm1,dm2,dm3 and dm4 are
                       dx1,dx2,dy1,dy2 where dx1,dy1  are the displacements
                       at the entrance of the element  along the axes x and
                       y.   The parameters dx2,dy2 are the displacements at
                       the exit of the element along the axes x and y

          option = 3   The element is misaligned around  the tangent to the
                       central trajectory  at the midpoint of  the element.
                       The  parameters dm1,dm2,dm3  and dm4  have the  same
                       meaning as in the case of the option value 1.

          option = 4   This does not apply to  dipole elements (Bends).  In
                       this  case  the  parameters dm1  and  dm3  represent
                       displacements in x and y respectively.  The particle
                       is also subjected at the entrance and the exit to an
                       angle kick of dm2/2 and dm/4 in x and y respectively
                       (same sign at  exit as at entrance).This  enables to
                       simulate baseline excursion in a similar way as that
                       defined under baseline operation.  Parameters dz dzr
                       are in  effect but  not ddel.This  should be  mainly
                       used on monitors.

------------------------------------------------------------------------

<span id="misalignment_data_definition"></span>
<span id="reference_orbit_display"></span>

## <span id="reference_orbit_display">REFERENCE ORBIT DISPLAY</span>

<span id="reference_orbit_display">This operation computes the orbit
defined by the initial coordinates x x' y y' al delta (delta=(p-p0/p0)),
and provides either a printout or a printer-plot display.</span>

Input format:

        REFErence ........(maximum 80 characters)
        nprint sizex sizey
        x  x'  y  y'  al delta
         0  0   0  0
        npos pos  .... pos
                1         npos

     Parameters:

        nprint      controls display
                1   a printout is provided
                2   a printer-plot is provided
               11 or 12 same as above , the orbit is s 4-dimensional closed
                    orbit defined by the variables x,x',y,y'
               21 or 22  same as  above ,  the  orbit is  a six-dimensional
                    closed orbit on the variables x,x',y,y',al,delta.

        sizex,sizey always  needed.   They  define  the boundaries  between
                    which the coodinates x and y are plotted.

        x0...delta  six  coordinates  of  the   input  particle  traced  to
                    determine the orbit.

       npos number of positions  selected for interval  computation maximum
                    4.

       pos(i)  the rms  values   are  computed  individually  in   all  the
                    intervals defined by the values 0,pos(i),end of lattice

------------------------------------------------------------------------

<span id="seed"></span>

## SEED

Using the clock of the computer, this operation generates and prints a
seed to be used in the random generators.

Input format:

                       SEED........(maximum 80 characters)
                       ns,

                    Parameters:

        ns      0   a seed is generated by the program.

            not 0   ns must be positive.   The program insures ns is an odd
                    number and prints the number used as the seed.

------------------------------------------------------------------------

<span id="seed"></span> <span id="set_corrector_values"></span>

## <span id="set_corrector_values">SET CORRECTOR VALUES</span>

<span id="set_corrector_values">This operation is used to manually set
corrector to some predetermined values Input format</span>

        SET Corrector values
        Name pos opt p1...p4,
        ....................
        Name pos opt p1...p4,
        99,

     Parameters:

        Name  Name of corrector element whose value is to be set

        pos   position of corrector

        opt   option number defining the kind of corrector

        p1...p4 the four parameters used to define the corrector.

        if opt = 0  p1  is  dx,p2 is  dy,p3  is  dy'  and  p4 is  ddel  The
                    corrector element is displaced uniformly  by dx and dy.
                    It is  preceded and  followed by  a momentum  dependent
                    kick  of dy'  (this  simulates  crudely the  effect  of
                    backleg windings providing a  Bx induction)  The energy
                    of the particle is changed  by ddel (This simulates the
                    effect of backleg windings providing a change in By)

           opt = 1  the parameters have the same definition as above.   The
                    displacement dx and  dy are imposed at  the entrance of
                    the magnet.   The exit point  of the magnet  is assumed
                    fixed. The operation of dy' and ddel remain the same as
                    above

           opt = 2  As for option  1 the parameters keep  their definition.
                    This time the  entrance is fixed and  the displacements
                    dx and  dy are imposed at  the exit of the  magnet.  In
                    both  cases 1  and 2  a momentum  independent slope  is
                    computed and imposed on the magnet.

           opt = 3  In  this option  the corrector  acts as  a pure  dipole
                    steering magnet.  Parameter 1 is dxp and parameter 2 is
                    dyp.  Parameter 3 and 4 are  not used.  The angle kicks
                    dxp and dyp are inversely  proportional to the momentum
                    of the partical(ie:  dxp and dyp are divided by 1+delta
                    where delta  is the relative  momentum of  the particle
                    traced.

------------------------------------------------------------------------

<span id="set_corrector_values"></span>
<span id="set_errors_of_elements"></span>

## <span id="set_errors_of_elements">SET ERRORS OF ELEMENTS</span>

<span id="set_errors_of_elements">This operation specifies the random
generation mode, the elements that should actually be affected by the
errors, and the intervals location in the beam line where they
lie.</span>

Input format:

                       SET Errors ....(maximum 80 characters)
                       nopt
                       nerr
                       name nint nb  nf ....nb    nf    ,
                                   1   1      nint  nint
                         ...........
                       name nint nb  nf ....nb    nf    ,
                                   1   1      nint  nint
     
                    Parameters:

The meaning of the parameters are the same as those of the SET MIS....
operation. See next operation. A special value of nopt : 5 , is used to
read the errors sequentially from the fortran input file f007. In this
case however one needs to know the order of the element parameters
internal to Dimad. Please contact Lindsay Schachinger to obtain the
information needed for correct use of this option.

------------------------------------------------------------------------

<span id="set_errors_of_elements"></span><span id="set_misalignment_of_elements"></span>

<span id="set_misalignment_of_elements"></span>

## <span id="set_misalignment_of_elements">SET MISALIGNMENT OF ELEMENTS</span>

<span id="set_misalignment_of_elements"></span>

Input format:

        SET Misalignment .....(maximum 80 characters)
        nopt
        nmis
        name nint nb  nf  ..... nb    nf
                    1   1         nint  nint
          .............
        name nint nb  nf  ..... nb    nf    ,
                    1   1         nint  nint

This operation defines the random generation mode, the elements that
should be actually misaligned, and the intervals location in the beam
line where they must be misaligned.

     Parameters:
        nopt        choice option for the random generators

               0    the elements are  misaligned by the fixed  values given
                    in the MISA... operation.  No randomness is introduced.

               1    The misalignment values are obtained by multiplying the
                    values given  in the  MISA...   operation  by +1  or -1
                    randomly generated.

                2   A  uniform distribution  is  generated  having the  rms
                    values defined by the MISA... operation.

                3   A Gaussian  distribution truncated  above two  standard
                    deviations is generated  with the rms value  defined by
                    the MISA... operation.

                4   A Gaussian  distribution truncated  above six  standard
                    deviations is generated  with the rms value  defined by
                    the MISA... operation.

                11,12,13,14 the random error is the  same as for 1,2,3 or 4
                    with a fast  random generation of the  random sequence.
                    This  random could,   in some  cases,   be affected  by
                    unwanted correlations.   In case  of doubt,   check the
                    STATISTICAL validity of  your results with a  family of
                    runs using the options 1,2,3 or 4.

        nmis        number of misaligned element type (names)

        name        name of the family of misaligned element

        nint        number of intervals in which element is misaligned
                0   all element  with that name  are misaligned.    In this
                    case no interval range is given.
               -1   no element with the name are misaligned.   In this case
                    no interval range is given.

        nb nf       beginning  and end  of  range  of misaligned  elements.
                    These numbers  correspond to  the order  number in  the
                    machine list.

------------------------------------------------------------------------

<span id="set_misalignment_of_elements"></span>
<span id="sho_correctors"></span>

## <span id="sho_correctors">SHO CORRECTORS</span>

<span id="sho_correctors">This operation displays the values of the
correctors and gives an elementary analysis of their values Input
format</span>

        SHO Correctors
        option             ....

     Parameters

     option   determines the information to be printed out.

               1    The rms, maxima and minima of the values are displayed

               2    name
                       This  option  prints  the  values 1  and  2  of  the
                    correctors  with the  label name  .   These values  are
                    multiplied by  the scale  factor SIGFAC  as defined  in
                    Constant definition.
     
               3    Under  this option  the values  of  the correctors  are
                    printed out in the format accepted  as input by the SET
                    Correctors operation.   This can be  used to set  up an
                    input file for a misaligned and corrected machine which
                    can be  then studied  without executing  the alignement
                    correction procedures.
                    SHOERRORS (not implemented yet)

------------------------------------------------------------------------

<span id="sho_correctors"></span> <span id="sho_misalignments"></span>

## <span id="sho_misalignments">SHO MISALIGNMENTS</span>

<span id="sho_misalignments">This operation displays the actual values
of the displacement generated in previous operations and provides
manipulation of the misalignement features. See demo6 for examples.
Input format</span>

                       SHO Misalignment
                       nrange ...............

                    Parameters

     nrange  0 then all misalignements are printed

               >0 then nrange intervals ni  mi are used.  The misalignments
               are printed in these intervals

               -1  then the  operation sets  up arrays  containing all  the
               misalignement data. Tracking execution proceeds faster.

               -2 name1 name1j1 name1k1 ... name1j5 name1k5,
                  name2 name2j1 name2k1 ... name2j5 name2k5,
                  ....
                  99,
                  MISFAC

Up to five names namei can be given and up to 5 intervals nameij nameik.
The names defining the intervals must be unique (use markers). The
operation adds to the elements namei a random misalignment as defined in
the misalignment data multiplied by the factor MISFAC but using a
different random sequence from that used in the main misalignement
procedure. This operation must be preceded by a SHO MIS operation with
option -1 to be successful.

                         -10 name

This operation provides information of the average lateral displacement
of the element labeled name using the scale factor SIGFAC. nrange number
of intervals in which the printout will occur. if nrange = -1, the the
operation sets up arrays containing all the misalignement data. Tracking
execution will then proceed faster(more space is needed) ni mi beginning
and end of interval in which printout occurs.

------------------------------------------------------------------------

<span id="sho_misalignments"></span>
<span id="synchrotron_radiation_data_definition"></span>

## <span id="synchrotron_radiation_data_definition">SYNCHROTRON RADIATION DATA DEFINITION</span>

<span id="synchrotron_radiation_data_definition">This operation computes
the energy losses due to synchrotron radiation in a deterministic way.It
only affects operation using particle tracing. Input format</span>

                  SYNChrotron radiation.....(maximum 80 characters)
                  Energy option randomoption,

               Parameters

        Energy   Initial nominal energy in GeV

        option   0 no synchrotron radiation effect is simulated.
                 1 synchrotron radiation loss is  computed in every dipole.
                    Particles lose that energy at  the entrance and exit of
                    the magnet.
                 2 synchrotron emittance  growth is simulated  randomly for
                    each particle.  This growth is due to the spread in the
                    energy loss of the particles.  This effect is simulated
                    only in the dipoles.
                 3 both the  radiation loss and  the emitttance  growth are
                    simulate in the dipoles only.
                 4 synchrotron radiation is simulated in  the dipoles as in
                    option  2  to  which is  added  a  synchrotron  quantum
                    fluctuation in the quadrupoles.
                 5 synchrotron radiation is simulated in  the dipoles as in
                    option  3  to  which is  added  a  synchrotron  quantum
                    fluctuation in the quadrupoles.

        randomoption:  choice  of the random  generator,  applies  to above
                    options 2 and 3 only.
                 1 the  random  generator  is  binary   +  and  -  randomly
                    affecting  the  energy spread  creating  the  emittance
                    growth.
                 2 the random generator is uniform
                 3 the random generator  is gaussian :  note  that here the
                    execution time  will be considerably greater  than with
                    choice 2 for the options 2 and 3 above which enable the
                    emittance growth calculations.
                11,12,13 the fast  random generator is used  to produce the
                    sequence  of  random  numbers.  This  sequence  may  be
                    affected  by some  unwanted correlations.   In case  of
                    doubt use the options 1,2 or 3.

------------------------------------------------------------------------

<span id="references"></span>

## REFERENCES:

\(1\) K.L.Brown,D.C.Carey,Ch.Iselin,F.Rothacker, TRANSPORT, A Computer
Program for Designing Charged Particle Beam Transport Systems SLAC 91
(1973 Rev.),NAL 91 and CERN 80-04.

\(2\) D. C. Carey and F. C. Iselin, "A Standard Input Language for
Particl Beam and Accelerator Computer Programs," Proceedings of the 1984
Summer Study on the Design and Utilization of the Superconducting Super
Collider, Snowmass, Colorado, June 1984. D. Douglas, L. Healy, F. C.
Iselin, and R. Ryne, "Report of the Group on a Common Input Format", SSC
Aperture Workshop Summary, SSC-TR-2001, Appendix 7 November 1984.

\(3\) F. Christoph Iselin, "The Mad Program Reference Manual," CERN, LEP
Division November 1, 1984.

\(4\) David Douglas, Etienne Forest, Roger Servranckx, A Method to
Render Second Order Beam Optics Programs Symplectic. LBL note SSC 28
LBL-18528. Also in the Proceedings of the 1985 Particle Accelerator
Conference, Vancouver.}

\(5\) Karl L. Brown, Roger V. Servranckx, First- and Second-Order
Charged Particle Optics. SLAC-PUB-3381. July 1984 .Stanford Linear
Accelerator Center.

------------------------------------------------------------------------

<span id="index"></span>

<span id="index"></span>

## INDEX

<span id="index"></span>[INTRODUCTION](#introduction)

[ELEMENT AND MACHINE DATA INPUT](#element_and_machine_data_input)

[UNITS](#units)

[GENERAL SYNTAX](#general_syntax)

[PARAMETERS](#parameters)

[ELEMENT DEFINITIONS](#element_definitions)

[ELEMENTS](#elements)

[BEAMLINE DEFINITIONS](#beamline_definitions)

[CONTROL FLOW](#control_flow)

[OPERATION LIST DESCRIPTION](#operation_list_description)

[IMPLEMENTED OPERATIONS](#implemented_operations)

[USE AND DESCRIPTION OF EACH](#use_and_description_of_each)

[ADIABATIC VARIATIONS](#adiabatic_variations)

[BEAM MATRIX TRACKING](#beam_matrix_tracking)

[CONSTANT DEFINITION](#constant_definition)

[DETAILED CHROMATIC ANALYSIS](#detailed_chromatic_analysis)

[GENERATION OF PARTICLES](#generation_of_particles)

[GEOMETRIC ABBERATIONS](#geometric_abberations)

[HARDWARE VALUES LISTING OF
MACHINE](#hardware_values_listing_of_machine)

[INTERACTIVE CONTROL OF LATTICE](#interactive_control_of_lattice)

[LEAST SQUARE FIT](#least_square_fit)

[LINE GEOMTRIC ABBERATIONS](#line_geomtric_abberations)

[MACHINE AND BEAM PARAMETERS
COMPUTATIONS](#machine_and_beam_parameters_computations)

[MATRIX COMPUTATION](#matrix_computation)

[MODIFICATION OF ELEMENT DATA](#modification_of_element_data)

[MOVEMENT ANALYSIS](#movement_analysis)

[OUTPUT CONTROL](#output_control)

[PARTICLE DISTRIBUTION ANALYSIS](#particle_distribution_analysis)

[PRINT SELECTION](#print_selection)

[PROGRAM GENERATION](#program_generation)

[RMATRIX COMPUTATION](#rmatrix_computation)

[SHO VALUES OF CONSTANTS](#sho_values_of_constants)

[SIMPLE FITTING](#simple_fitting)

[SEISMIC PERTURBATION SIMULATION](#seismic_perturbation_simulation)

[SET FIT POINT](#set_fit_point)

[SET LIMITS TO VARIABLES](#set_limits_to_variables)

[SET SYMPLETIC OPTION ON](#set_symplectic_option_on)

[SPACE CHARGE COMPUTATION](#space_charge_computation)

[TRACKING OF PARTICLES](#tracking_of_particles)

[OPERATIONS USED IN CONJUCTION](#operations_used_in_conjuction)

[ALIGNMENT FITTING](#alignment_fitting)

[BASELINE DEFINITION](#baseline_definition)

[BLOCK MISALIGNMENT](#block_misalignment)

[CORRECTOR DATA DEFINITION](#corrector_data_definition)

[ERRORS DATA DEFINITION](#errors_data_definition)

[MISALIGNMENT DATA DEFINITION](#misalignment_data_definition)

[REFERENCE ORBIT DISPLAY](#reference_orbit_display)

[SEED](#seed)

[SET CORRECTOR VALUES](#set_corrector_values)

[SET ERRORS OF ELEMENTS](#set_errors_of_elements)

[SET MISALIGNMENT OF ELEMENTS](#set_misalignment_of_elements)

[SHO CORRECTORS](#sho_correctors)

[SHO MISALIGNMENTS](#sho_misalignments)

[SYNCHROTRON RADIATION DATA
DEFINITION](#synchrotron_radiation_data_definition)

[REFERENCES](#references)

------------------------------------------------------------------------

Translated to HMTL by KBB, 14may04

------------------------------------------------------------------------

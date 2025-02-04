2025-02-01T12:38:30-08:00
Been at this for > 1 h.

Going thru the example
- Hyperelastic Seal
The example ran fine, however when I build it from sratch it dod not work.

Warning:
```
PARDISO not supported on this system. Switching to MUMPS.

MUMPS memory allocation factor increased to 1.4399999999999999.
```

Error:
```
Failed to find a solution for the initial parameter.

No convergence, relative stepsize too small.

Returned solution is not converged.

Not all parameter steps returned.
```


Seems like OS related??
https://www.comsol.com/support/knowledgebase/1300

2025-02-01T12:44:34-08:00
Ugh I have to leave for lunch.

# Comparing my own hyperelastic seal vs the one from the library
2025-02-03T21:56:40-08:00

Sent to deepseek.

```
diff --git a/File1.m b/File2.m
index 1234567..89abcde 100644
--- a/File1.m
+++ b/File2.m
@@ -1,9 +1,9 @@
 function out = model
 %
-% hyperelastic_seal.m
+% test_hyperelastic.m
 %
-% Model exported on Feb 3 2025, 21:54 by COMSOL 6.2.0.339.
+% Model exported on Feb 3 2025, 21:52 by COMSOL 6.2.0.339.
 
 import com.comsol.model.*
 import com.comsol.model.util.*
 
@@ -12,9 +12,7 @@
 
 model.modelPath('/Users/wentaojiang/Documents/COMSOL/20250201_moving_BC_debug');
 
-model.label('hyperelastic_seal.mph');
-
-model.title('Hyperelastic Seal');
+model.label('test_hyperelastic.mph');
 
 model.description('A seal is modeled using a hyperelastic material model with large deformations. The model compares the stiffness with and without internal pressure.');
 
@@ -46,7 +44,6 @@
 model.component('comp1').geom('geom1').feature('thi1').set('upthick', 1.5);
 model.component('comp1').geom('geom1').feature('thi1').selection('input').set({'fil2'});
 model.component('comp1').geom('geom1').create('c1', 'Circle');
-model.component('comp1').geom('geom1').feature('c1').label('Indenter');
 model.component('comp1').geom('geom1').feature('c1').set('selresult', true);
 model.component('comp1').geom('geom1').feature('c1').set('selresultshow', 'bnd');
 model.component('comp1').geom('geom1').feature('c1').set('pos', [4 24]);
@@ -60,7 +57,7 @@
 model.component('comp1').geom('geom1').create('r3', 'Rectangle');
 model.component('comp1').geom('geom1').feature('r3').set('contributeto', 'csel1');
 model.component('comp1').geom('geom1').feature('r3').set('pos', [13 0]);
-model.component('comp1').geom('geom1').feature('r3').set('size', [1 12]);
+model.component('comp1').geom('geom1').feature('r3').set('size', [1 23]);
 model.component('comp1').geom('geom1').create('ccur1', 'ConvertToCurve');
 model.component('comp1').geom('geom1').feature('ccur1').selection('input').set({'c1' 'r2' 'r3'});
 model.component('comp1').geom('geom1').create('del1', 'Delete');
@@ -92,7 +89,7 @@
 model.component('comp1').geom('geom1').feature('sel3').selection('selection').init(1);
 model.component('comp1').geom('geom1').feature('sel3').selection('selection').set('fin(1)', 4);
 model.component('comp1').geom('geom1').nodeGroup.create('grp1');
-model.component('comp1').geom('geom1').nodeGroup('grp1').label('Seal');
+model.component('comp1').geom('geom1').nodeGroup('grp1').label('seal');
 model.component('comp1').geom('geom1').nodeGroup('grp1').placeAfter([]);
 model.component('comp1').geom('geom1').nodeGroup('grp1').add('r1');
 model.component('comp1').geom('geom1').nodeGroup('grp1').add('fil1');
@@ -117,8 +114,8 @@
 
 model.component('comp1').common.create('pres1', 'PrescribedDeformation');
 model.component('comp1').common('pres1').selection.named('geom1_c1_bnd');
+model.component('comp1').common('pres1').selection.geom('geom1', 1);
+model.component('comp1').common('pres1').selection.set([3]);
 
 model.component('comp1').physics.create('solid', 'SolidMechanics', 'geom1');
 model.component('comp1').physics('solid').feature('dcnt1').create('fric1', 'Friction', 1);
@@ -141,10 +138,10 @@
 model.component('comp1').mesh('mesh1').feature('fq1').feature('size2').selection.named('geom1_sel2');
 
 model.component('comp1').view('view1').axis.set('xmin', -10.534534454345703);
-model.component('comp1').view('view1').axis.set('xmax', 16.534534454345703);
-model.component('comp1').view('view1').axis.set('ymin', -0.7669525146484375);
-model.component('comp1').view('view1').axis.set('ymax', 16.08051300048828);
-
+model.component('comp1').view('view1').axis.set('xmax', 24.902278900146484);
+model.component('comp1').view('view1').axis.set('ymin', -2.013140916824341);
+model.component('comp1').view('view1').axis.set('ymax', 22.136940002441406);
+
+model.component('comp1').pair('p1').label('contact pair - upper');
 model.component('comp1').pair('p1').pairName('upper');
 model.component('comp1').pair('p2').pairName('lower');
 
@@ -183,7 +180,7 @@
 model.sol('sol1').feature('s1').create('fc1', 'FullyCoupled');
 model.sol('sol1').feature('s1').feature.remove('fcDef');
 
-model.result.create('pg1', 'PlotGroup2D');
+model.result.create('pg1', 'PlotGroup2D');
 model.result.create('pg2', 'PlotGroup2D');
 model.result.create('pg3', 'PlotGroup2D');
 model.result.create('pg4', 'PlotGroup1D');
@@ -212,7 +209,7 @@
 model.study('std1').feature('stat').set('plot', true);
 model.study('std1').feature('stat').set('useparam', true);
 model.study('std1').feature('stat').set('pname', {'para'});
-model.study('std1').feature('stat').set('plistarr', {'1e-3 range(0.1,0.1,4)'});
+model.study('std1').feature('stat').set('plistarr', {'1e-3*range(0.1,0.1,4)'});
 model.study('std1').feature('stat').set('punit', {''});
 model.study('std1').feature('stat').set('useloadcase', true);
 model.study('std1').feature('stat').set('loadcase', {'Without pressure' 'With pressure'});
@@ -229,6 +226,7 @@
 model.sol('sol1').feature('v1').set('clistctrl', {'p1'});
 model.sol('sol1').feature('v1').set('cname', {'para'});
 model.sol('sol1').feature('v1').set('clist', {'1e-3 range(0.1,0.1,4)'});
+model.sol('sol1').feature('v1').set('clist', {'1e-3*range(0.1,0.1,4)'});
 model.sol('sol1').feature('v1').feature('comp1_solid_hmm1_pw').set('scalemethod', 'manual');
 model.sol('sol1').feature('v1').feature('comp1_solid_hmm1_pw').set('scaleval', '1e5');
 model.sol('sol1').feature('v1').feature('comp1_u').set('scalemethod', 'manual');
@@ -237,6 +235,8 @@
 model.sol('sol1').feature('v1').feature('comp1_solid_enc1_fl1_lm').set('scaleval', 0.001);
 model.sol('sol1').feature('s1').label('Stationary Solver 1.1');
 model.sol('sol1').feature('s1').set('probesel', 'none');
+model.sol('sol1').feature('s1').set('stol', 0.01);
+model.sol('sol1').feature('s1').feature('dDef').set('pivotperturb', 1.0E-7);
 model.sol('sol1').feature('s1').feature('dDef').label('Direct 1');
 model.sol('sol1').feature('s1').feature('dDef').set('linsolver', 'pardiso');
 model.sol('sol1').feature('s1').feature('aDef').label('Advanced 1');
@@ -260,65 +260,11 @@
 model.result('pg1').label('Stress (solid)');
 model.result('pg1').set('frametype', 'spatial');
 model.result('pg1').feature('surf1').set('const', {'solid.refpntx' '0' 'Reference point for moment computation, x-coordinate'; 'solid.refpnty' '0' 'Reference point for moment computation, y-coordinate'; 'solid.refpntz' '0' 'Reference point for moment computation, z-coordinate'});
-model.result('pg1').feature('surf1').set('colortable', 'Prism');
-model.result('pg1').feature('surf1').set('threshold', 'manual');
-model.result('pg1').feature('surf1').set('thresholdvalue', 0.2);
-model.result('pg1').feature('surf1').set('resolution', 'normal');
-model.result('pg1').feature('arwl1').set('expr', {'solid.enc1.F_Ax' 'solid.enc1.F_Ay'});
-model.result('pg1').feature('arwl1').set('descr', 'Load (material and geometry frames)');
-model.result('pg1').feature('arwl1').set('const', {'solid.refpntx' '0' 'Reference point for moment computation, x-coordinate'; 'solid.refpnty' '0' 'Reference point for moment computation, y-coordinate'; 'solid.refpntz' '0' 'Reference point for moment computation, z-coordinate'});
-model.result('pg1').feature('arwl1').set('titletype', 'none');
-model.result('pg1').feature('arwl1').set('arrowcount', 300);
-model.result('pg1').feature('arwl1').set('arrowbase', 'head');
-model.result('pg1').feature('arwl1').set('scale', 1.8138778096155553E-5);
-model.result('pg1').feature('arwl1').set('inheritplot', 'surf1');
-model.result('pg1').feature('arwl1').set('inheritcolor', false);
-model.result('pg1').feature('arwl1').set('inheritrange', false);
-model.result('pg1').feature('arwl1').set('scaleactive', false);
 model.result('pg2').label('Moving Mesh');
 model.result('pg2').feature('mesh1').set('qualmeasure', 'custom');
 model.result('pg2').feature('mesh1').set('qualexpr', 'comp1.spatial.relVol');
 model.result('pg2').feature('mesh1').set('colorrangeunitinterval', false);
 model.result('pg2').feature('mesh1').set('colortable', 'TrafficFlow');
-model.result('pg2').feature('mesh1').set('colortabletrans', 'nonlinear');
-model.result('pg2').feature('mesh1').set('nonlinearcolortablerev', true);
-model.result('pg2').feature('mesh1').set('resolution', 'normal');
-model.result('pg3').label('Contact Forces (solid)');
-model.result('pg3').set('titletype', 'label');
-model.result('pg3').set('frametype', 'spatial');
-model.result('pg3').set('showlegendsunit', true);
-model.result('pg3').set('legendpos', 'rightdouble');
-model.result('pg3').feature('arwl1').label('Contact 1, Pressure');
-model.result('pg3').feature('arwl1').set('expr', {'solid.dcnt1.Tnx' 'solid.dcnt1.Tny'});
-model.result('pg3').feature('arwl1').set('descr', 'Contact pressure (spatial frame)');
-model.result('pg3').feature('arwl1').set('const', {'solid.refpntx' '0' 'Reference point for moment computation, x-coordinate'; 'solid.refpnty' '0' 'Reference point for moment computation, y-coordinate'; 'solid.refpntz' '0' 'Reference point for moment computation, z-coordinate'});
-model.result('pg3').feature('arwl1').set('placement', 'gausspoints');
-model.result('pg3').feature('arwl1').set('gporder', 4);
-model.result('pg3').feature('arwl1').set('arrowbase', 'head');
-model.result('pg3').feature('arwl1').set('scale', 1.486004210749807E-6);
-model.result('pg3').feature('arwl1').set('scaleactive', false);
-model.result('pg3').feature('arwl1').feature('col').set('colordata', 'arrowlength');
-model.result('pg3').feature('arwl1').feature('col').set('coloring', 'gradient');
-model.result('pg3').feature('arwl1').feature('col').set('topcolor', 'green');
-model.result('pg3').feature('arwl1').feature('col').set('bottomcolor', 'custom');
-model.result('pg3').feature('arwl1').feature('col').set('custombottomcolor', [0.509804 0.54902 0.509804]);
-model.result('pg3').feature('arwl2').label('Contact 1, Friction Force');
-model.result('pg3').feature('arwl2').set('expr', {'solid.dcnt1.Ttx' 'solid.dcnt1.Tty'});
-model.result('pg3').feature('arwl2').set('descr', 'Friction force (spatial frame)');
-model.result('pg3').feature('arwl2').set('const', {'solid.refpntx' '0' 'Reference point for moment computation, x-coordinate'; 'solid.refpnty' '0' 'Reference point for moment computation, y-coordinate'; 'solid.refpntz' '0' 'Reference point for moment computation, z-coordinate'});
-model.result('pg3').feature('arwl2').set('placement', 'gausspoints');
-model.result('pg3').feature('arwl2').set('gporder', 4);
-model.result('pg3').feature('arwl2').set('scale', 4.9533473691660225E-6);
-model.result('pg3').feature('arwl2').set('scaleactive', false);
-model.result('pg3').feature('arwl2').feature('col').set('colordata', 'arrowlength');
-model.result('pg3').feature('arwl2').feature('col').set('coloring', 'gradient');
-model.result('pg3').feature('arwl2').feature('col').set('topcolor', 'magenta');
-model.result('pg3').feature('arwl2').feature('col').set('bottomcolor', 'custom');
-model.result('pg3').feature('arwl2').feature('col').set('custombottomcolor', [0.54902 0.509804 0.54902]);
-model.result('pg3').feature('surf1').label('Gray Surfaces');
-model.result('pg3').feature('surf1').set('const', {'solid.refpntx' '0' 'Reference point for moment computation, x-coordinate'; 'solid.refpnty' '0' 'Reference point for moment computation, y-coordinate'; 'solid.refpntz' '0' 'Reference point for moment computation, z-coordinate'});
-model.result('pg3').feature('surf1').set('coloring', 'uniform');
-model.result('pg3').feature('surf1').set('color', 'gray');
-model.result('pg3').feature('surf1').set('resolution', 'normal');
-model.result('pg4').label('Contact Pressure');
-model.result('pg4').set('looplevelinput', {'manualindices' 'last'});
-model.result('pg4').set('looplevelindices', {'range(10,5,40)' ''});
-model.result('pg4').set('titletype', 'manual');
-model.result('pg4').set('title', 'Contact pressure profile');
-model.result('pg4').set('xlabel', 'Arc length (mm)');
-model.result('pg4').set('ylabel', 'Contact pressure (kPa)');
-model.result('pg4').set('xlabelactive', false);
-model.result('pg4').set('ylabelactive', false);
-model.result('pg4').feature('lngr1').set('unit', 'kPa');
-model.result('pg4').feature('lngr1').set('const', {'solid.refpntx' '0' 'Reference point for moment computation, x-coordinate'; 'solid.refpnty' '0' 'Reference point for moment computation, y-coordinate'; 'solid.refpntz' '0' 'Reference point for moment computation, z-coordinate'});
-model.result('pg4').feature('lngr1').set('linewidth', 2);
-model.result('pg4').feature('lngr1').set('linewidthslider', 2);
-model.result('pg4').feature('lngr1').set('legend', true);
-model.result('pg4').feature('lngr1').set('autosolution', false);
-model.result('pg4').feature('lngr1').set('legendprefix', 'eval(para)');
-model.result('pg4').feature('lngr1').set('resolution',
```

It is fucking this line:
![[Pasted image 20250203220643.png]]
- What the fuck??
- ok it is because it reduced the step size
- Why would the param step made the solver change??
![[Pasted image 20250203223623.png]]
![[Pasted image 20250203223629.png]]

# Back to the solid seal model
2025-02-03T22:36:32-08:00

Suspecting it is because of the 2D axial symmetry.
Built just simple 2D model. Did not work, same problem.
Also suspected mesh not fine enough, refined. Same problem.
Also suspected the flat line is not touching the seal. Changed it to be touching initially. Same problem...
![[Pasted image 20250203223747.png]]

2025-02-03T22:51:01-08:00
change the indenter in the example to be a straight line, it also no longer works and just goes right thru
![[Pasted image 20250203225106.png]]

2025-02-03T23:54:49-08:00
Created an even simpler toy model, not working
![[Pasted image 20250203235551.png]]

What the fuck, I had to make the circle as solid first, and then convert to curve, instead of directly making it a curve, and then it works:
![[Pasted image 20250203235755.png]]

2025-02-04T00:06:41-08:00
Mario's model is also working:
![[Pasted image 20250204000649.png]]

I could finally sleep in peace.


## Summary of important unusual changes

- Form union:
	- choose 'form assembly'
	- Clear the Create pairs check box.





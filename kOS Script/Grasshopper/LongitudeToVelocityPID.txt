Set errorLngV to coordinates:lng - longitude.
Set derivativeLngV to (previousLng - longitude) / dt.
Set previousLng to longitude.
Set integralLngV to integralLngV + (errorLngV * dt).
If (integralLngV * KiLngV ) > outMaxLngV{
	Set integralLngV to outMaxLngV / KiLngV.
}.
If (integralLngV * KiLngV) < outMinLngV{
	Set integralLngV to outMinLngV / KiLngV.
}.
Set outLngV to ((KpLngV * errorLngV) + (KiLngV * integralLngV) + (KdLngV * derivativeLngV)).
If (outLngV > outMaxLngV){
	Set outLngV to outMaxLngV.
}.
If (outLngV < outMinLngV){
	Set outLngV to outMinLngV.
}.
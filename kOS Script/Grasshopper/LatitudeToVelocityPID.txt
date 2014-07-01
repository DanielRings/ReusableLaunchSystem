Set errorLatV to coordinates:lat - latitude.
Set derivativeLatV to (previousLat - latitude) / dt.
Set previousLat to latitude.
Set integralLatV to integralLatV + (errorLatV * dt).
If (integralLatV * KiLatV ) > outMaxLatV{
	Set integralLatV to outMaxLatV / KiLatV.
}.
If (integralLatV * KiLatV) < outMinLatV{
	Set integralLatV to outMinLatV / KiLatV.
}.
Set outLatV to ((KpLatV * errorLatV) + (KiLatV * integralLatV) + (KdLatV * derivativeLatV)).
If (outLatV > outMaxLatV){
	Set outLatV to outMaxLatV.
}.
If (outLatV < outMinLatV){
	Set outLatV to outMinLatV.
}.
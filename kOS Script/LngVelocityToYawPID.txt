Declare parameter desiredLngVelocity.
Set errorVY to desiredLngVelocity - velLng.
Set integralVY to integralVY + (errorVY * dt).
If (integralVY * KiVY ) > outMaxVY{
	Set integralVY to outMaxVY / KiVY.
}.
If (integralVY * KiVY) < outMinVY{
	Set integralVY to outMinVY / KiVY.
}.
Set derivativeVY to (velLng - previousVelLng) / dt.
Set outVY to ((KpVY * errorVY) + (KiVY * integralVY) + (KdVY * derivativeVY)) * -1.
If (outVY > outMaxVY){
	Set outVY to outMaxVY.
}.
If (outVY < outMinVY){
	Set outVY to outMinVY.
}.
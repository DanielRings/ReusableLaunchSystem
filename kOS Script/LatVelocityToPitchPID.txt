Declare parameter desiredLatVelocity.
Set errorVP to desiredLatVelocity - velLat.
Set integralVP to integralVP + (errorVP * dt).
If (integralVP * KiVP ) > outMaxVP{
	Set integralVP to outMaxVP / KiVP.
}.
If (integralVP * KiVP) < outMinVP{
	Set integralVP to outMinVP / KiVP.
}.
Set derivativeVP to (velLat - previousVelLat) / dt.
Set outVP to ((KpVP * errorVP) + (KiVP * integralVP) + (KdVP * derivativeVP)) * -1.
If (outVP > outMaxVP){
	Set outVP to outMaxVP.
}.
If (outVP < outMinVP){
	Set outVP to outMinVP.
}.
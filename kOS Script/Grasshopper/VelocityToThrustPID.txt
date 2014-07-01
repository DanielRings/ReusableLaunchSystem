Declare parameter desiredVelocity.
Set errorVT to desiredVelocity - verticalspeed.
Set integralVT to integralVT + (errorVT * dt).
If (integralVT * KiVT ) > outMaxVT{
	Set integralVT to outMaxVT / KiVT.
}.
If (integralVT * KiVT) < outMinVT{
	Set integralVT to outMinVT / KiVT.
}.
Set derivativeVT to (verticalspeed - previousV) / dt.
Set previousV to verticalspeed.
Set outVT to ((KpVT * errorVT) + (KiVT * integralVT) + (KdVT * derivativeVT)).
If (outVT > outMaxVT){
	Set outVT to outMaxVT.
}.
If (outVT < outMinVT){
	Set outVT to outMinVT.
}.
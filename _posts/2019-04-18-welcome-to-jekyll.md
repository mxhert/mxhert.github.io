---
title: "VQ R200 Final Drive Ratio Calculator"
date: 2025-03-13T15:34:30-04:00
categories:
  - blog
tags:
  - Z1
  - Resources
---

<!DOCTYPE html>
<html>
<head>
    <script>
        function calculateSpeeds() {
            const redline = parseFloat(document.getElementById('redline').value);
            const width = parseFloat(document.getElementById('width').value);
            const aspectRatio = parseFloat(document.getElementById('aspectRatio').value);
            const wheelDiameter = parseFloat(document.getElementById('wheelDiameter').value);
            const tireDiameter = wheelDiameter + 2 * (width / 25.4) * (aspectRatio / 100);
            const constant = 336;
            
            const manualRatios = [3.794, 2.324, 1.624, 1.271, 1, 0.794];
            const auto7Ratios = [4.924, 3.194, 2.043, 1.412, 1, 0.862, 0.772];
			const auto5Ratios = [3.842, 2.353, 1.529, 1, 0.839];

            
            const transmissionType = document.getElementById("transmission").value;
            const beforeFinalDrive = parseFloat(document.getElementById("beforeFinalDrive").value);
            const afterFinalDrive = parseFloat(document.getElementById("afterFinalDrive").value);
            
            const outputBefore = document.getElementById("outputBefore");
            const outputAfter = document.getElementById("outputAfter");
            const cruisingRPMBefore = document.getElementById("cruisingRPMBefore");
            const cruisingRPMAfter = document.getElementById("cruisingRPMAfter");
            
            outputBefore.innerHTML = "";
            outputAfter.innerHTML = "";
            
            let gearRatios = transmissionType === "manual" ? manualRatios : transmissionType === "auto7" ? auto7Ratios : auto5Ratios;
            
            gearRatios.forEach((gearRatio, index) => {
                let topSpeedBefore = (redline * tireDiameter) / (beforeFinalDrive * gearRatio * constant);
                let topSpeedAfter = (redline * tireDiameter) / (afterFinalDrive * gearRatio * constant);
                outputBefore.innerHTML += `Gear ${index + 1}: ${topSpeedBefore.toFixed(2)} mph<br>`;
                outputAfter.innerHTML += `Gear ${index + 1}: ${topSpeedAfter.toFixed(2)} mph<br>`;
            });
            
            let highestGearRatio = gearRatios[gearRatios.length - 1];
            let cruisingRPMBeforeValue = (75 * highestGearRatio * beforeFinalDrive * constant) / tireDiameter;
            let cruisingRPMAfterValue = (75 * highestGearRatio * afterFinalDrive * constant) / tireDiameter;
            
            cruisingRPMBefore.innerHTML = `Engine Speed at 75 mph: ${cruisingRPMBeforeValue.toFixed(2)} RPM`;
            cruisingRPMAfter.innerHTML = `Engine Speed at 75 mph: ${cruisingRPMAfterValue.toFixed(2)} RPM`;
        }
    </script>
</head>
<body onload="calculateSpeeds()">
    <h2>Z1 Final Drive Ratio Comparison Calculator</h2>
	
	<label for="width">Rear Tire Size:</label>
    <input type="number" id="width" value="275" style="width: 5ch;"/>
	<label for="aspectRatio">/</label>
    <input type="number" id="aspectRatio" value="35" style="width: 4ch;"/>
    <label for="wheelDiameter">R</label>
    <input type="number" id="wheelDiameter" value="19" style="width: 4ch;"/>
	<br />
	
    <label for="transmission">Transmission:</label>
    <select id="transmission">
        <option value="manual">6-Speed Manual</option>
        <option value="auto7">7-Speed Automatic</option>
        <option value="auto5">5-Speed Automatic</option>
    </select>
    <script>
        document.getElementById('transmission').addEventListener('change', function() {
            if (this.value === 'manual') {
                document.getElementById('beforeFinalDrive').value = '3.54';
            }
			 if (this.value === 'auto7') {
                document.getElementById('beforeFinalDrive').value = '3.36';
            }
			 if (this.value === 'auto5') {
                document.getElementById('beforeFinalDrive').value = '3.36';
            }
        });
    </script>
    <br />
	
	<label for="redline">Redline (RPM):</label>
    <input type="number" id="redline" value="7500" style="width: 6ch;"/>
	  <br />
	  <br />
	  

    

    
    <button onclick="calculateSpeeds()">Recalculate</button>
    
    <div style="display: flex; gap: 20px;">
        <div>
		    <label for="beforeFinalDrive">ORIGINAL Final Drive Ratio:</label>
    <select id="beforeFinalDrive">
        <option value="3.36">3.36</option>
        <option value="3.54" selected>3.54</option>
        <option value="3.69">3.69</option>
        <option value="3.91">3.90</option>
        <option value="4.09">4.09</option>
        <option value="4.36">4.36</option>
    </select>
            <h4>Top Speed Per Gear:</h4>
            <div id="outputBefore"></div>
            <h4>Cruising RPM:</h4>
            <div id="cruisingRPMBefore"></div>
        </div>
        <div>
		    <label for="afterFinalDrive">NEW Final Drive Ratio:</label>
    <select id="afterFinalDrive">
        <option value="3.36">3.36</option>
        <option value="3.54">3.54</option>
        <option value="3.69">3.69</option>
        <option value="3.91">3.90</option>
        <option value="4.09" selected>4.09</option>
        <option value="4.36">4.36</option>
    </select>
            <h4>Top Speed Per Gear:</h4>
            <div id="outputAfter"></div>
            <h4>Cruising RPM:</h4>
            <div id="cruisingRPMAfter"></div>
        </div>
    </div>
</body>
</html>

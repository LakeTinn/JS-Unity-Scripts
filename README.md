JS-Unity-Scripts
================
var shotSound : AudioClip; // drag a shot sound here, if any
var outSound : AudioClip;
var reloadSound : AudioClip;

var bloodPrefab : GameObject; // drag the blood prefab here, if any
var sparksPrefab : GameObject; // drag the sparks prefab here, if any
var fireAttackPrefab : GameObject;

var curAmmo1 : int = 30;
var maxAmmo1 : int = 30;
var magCount1 : int = 3;
var curAmmo2 : int = 45;
var maxAmmo2 : int = 45;
var magCount2 : int = 5;
var curAmmo3 : int = 60;
var maxAmmo3 : int = 60;
var magCount3 : int = 10;
var score : int = 1000;
var damage : int = 100;

var nextFireTime : float;
var fireRate : float = 0.25f;

var ammotext1 : GUIText;
var upgradeText : GUIText;
var scoreText : GUIText;
var uprgradeText : GUIText;

private var curWeapon : int = 1;
private var wep1Own : boolean = true;
private var wep2Own : boolean = false;
private var wep3Own : boolean = false;

function Update () {
//Score System
scoreText.text = "Current Score: " + score;
if (score >= 750) {
upgradeText.text = "Can upgrade:Yes";
}
else{
upgradeText.text = "Can upgrade:No";
}
//Fire Weapon
		
		//Displays Current Ammo and Current Magazine count for weapon 1
		if(curWeapon == 1){
		ammotext1.text = "Weapon 1: " + curAmmo1 + " / " + magCount1; 
		}
		
		//Displays Current Ammo and Current Magazine count for weapon 2
		if(curWeapon == 2){
		ammotext1.text = "Weapon 2: " + curAmmo2 + " / " + magCount2; 
		}
		
		//Displays Current Ammo and Current Magazine count for weapon 3
		if(curWeapon == 3){
		ammotext1.text = "Weapon 3: " + curAmmo3 + " / " + magCount3; 
		}
		
		//Choose previous weapon
		if(Input.GetKeyDown("q") && curWeapon != 1){
			curWeapon--;
		}
		
		//Choose next weapon for wep 2 if unowned
		if(Input.GetKeyDown("e") && curWeapon == 1 && score >= 750 && wep2Own == false){
			curWeapon++;
			score -= 750;
			wep2Own = true;
		}
		
		//Choose next weapon for wep 3 if unowned
		if(Input.GetKeyDown("e") && curWeapon == 2 && score >= 1000 && wep3Own ==false){
			curWeapon++;
			score -= 1000;
			wep3Own = true;
		}
		
		//Choose next weapon for wep 2 if owned
		if(Input.GetKeyDown("e") && curWeapon == 1 && wep2Own == true){
			curWeapon++;
		}
		
		//Choose next weapon for wep 3 if owned
		if(Input.GetKeyDown("e") && curWeapon == 2 && wep3Own == true){
			curWeapon++;
		}
		
		//Fixes negative ammo ammounts for weapon 1
		if(curAmmo1 < 0 ) {
			curAmmo1 = 0;
		}
		
		//Fixes negative ammo ammounts for weapon 2
		if(curAmmo2 < 0 ) {
			curAmmo2 = 0;
		}
		
		//Fixes negative ammo ammounts for weapon 3
		if(curAmmo3 < 0 ) {
			curAmmo3 = 0;
		}
		
	//Shooting for weapon 1
     if(Input.GetMouseButtonDown(0) && curAmmo1 > 0 && curWeapon == 1){
   		curAmmo1--;
   		nextFireTime = Time.time + fireRate;
   		ForceFire();
      }
    
    //Shooting for weapon 2
     if(Input.GetMouseButton(0) && curAmmo2 > 0 && curWeapon == 2 && Time.time >= nextFireTime){
   		curAmmo2--;
   		nextFireTime = Time.time + fireRate;
   		ForceFire();
      }
    
    //Shooting for weapon 3
     if(Input.GetMouseButton(0) && curAmmo3 > 0 && curWeapon == 3 && Time.time > nextFireTime){
   		curAmmo3--;
   		nextFireTime = Time.time + fireRate;
   		ForceFire();
      }
      
    //Plays audio for empty weapon 1
	if(Input.GetMouseButtonDown(0) && curAmmo1 == 0 && curWeapon == 1){
		audio.PlayOneShot(outSound);
    }
    
    
    //Plays audio for empty weapon 2
	if(Input.GetMouseButtonDown(0) && curAmmo2 == 0 && curWeapon == 2){
		audio.PlayOneShot(outSound);
    }
    
    //Plays audio for empty weapon 3
	if(Input.GetMouseButtonDown(0) && curAmmo3 == 0 && curWeapon == 3){
		audio.PlayOneShot(outSound);
    }
    
    
    //Reloads Weapon 1
    if(Input.GetKeyDown("r") && magCount1 > 0 && curWeapon == 1) {
		curAmmo1 = 30;
		magCount1--;
		audio.PlayOneShot(reloadSound);
	}
	
	//Reloads Weapon 2
    if(Input.GetKeyDown("r") && magCount2 > 0 && curWeapon == 2) {
		curAmmo2 = maxAmmo2;
		magCount2--;
		audio.PlayOneShot(reloadSound);
	}
	//Reloads Weapon 3
    if(Input.GetKeyDown("r") && magCount3 > 0 && curWeapon == 3) {
		curAmmo3 = maxAmmo3;
		magCount3--;
		audio.PlayOneShot(reloadSound);
	}
	}
	
	function ForceFire () {
		audio.PlayOneShot(shotSound); // play the shot sound
    	var hit : RaycastHit;
    	var ray : Ray = Camera.main.ScreenPointToRay(Vector3(Screen.width * 0.5, Screen.height * 0.5, 0));
        if (Physics.Raycast(ray, hit, 100)){
        	var particleClone = Instantiate(bloodPrefab, hit.point, Quaternion.LookRotation(hit.normal));
        	Destroy(particleClone.gameObject, 2);
        	hit.transform.SendMessage("ApplyDamage", damage, SendMessageOptions.DontRequireReciever);
        }
        				}

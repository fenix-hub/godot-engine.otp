# gd-otp
Godot Engine plugin to generate ![RFC4226](https://www.rfc-editor.org/rfc/rfc4226) and ![RFC6238](https://www.rfc-editor.org/rfc/rfc6238) compliant One Time Passwords in GDScript. 

HOTP (RFC4226) and TOTP (RFC6238) are mostly used for ![Multi Factor Authentication (MFA)](https://en.wikipedia.org/wiki/Multi-factor_authentication) and Two Factor Authentication (2FA).
Generated TOTP are compatible with Microsoft Authenticator, Google Authenticator, and most used 2fa apps.

This plugin also contains a Base32 Encoder/Decoder, which **may** be required to decode Base32 shared secrets provided by most platforms (Github, Google, Microsoft, etc...), used to generate a Time-based One Time Password (TOTP).


### HOTP example
```gdscript
func _ready() -> void:
	var secret: PoolByteArray == "secret".to_ascii()
	var moving_factor: int = randi() % 1000
	var hotp_generator: HOTPGenerator = HOTPGenerator.new()
	var hotp: String = hotp_generator.generate_hotp(secret, moving_factor)
	print(hotp) # will print something similar to `370727`
```

### TOTP example
```gdscript
func _ready() -> void:
	var secret: PoolByteArray == "secret".to_ascii()
	var totp_generator: TOTPGenerator = TOTPGenerator.new()
	var totp: String = totp_generator.generate_totp(secret)
	print(totp_generator.secs_remaining) # will print something between 0 and 30
	print(totp)	# will print something similar to `183920`
```

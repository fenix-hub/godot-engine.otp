# gd-otp
Godot Engine plugin to generate ![RFC4226](https://www.rfc-editor.org/rfc/rfc4226) and ![RFC6238](https://www.rfc-editor.org/rfc/rfc6238) compliant One Time Passwords in GDScript. 

HOTP (RFC4226) and TOTP (RFC6238) are mostly used for ![Multi Factor Authentication (MFA)](https://en.wikipedia.org/wiki/Multi-factor_authentication) and Two Factor Authentication (2FA).
Generated TOTP are compatible with Microsoft Authenticator, Google Authenticator, and most used 2fa apps.

This plugin also contains a Base32 Encoder/Decoder, which **may** be required to decode Base32 shared secrets provided by most platforms (Github, Google, Microsoft, etc...), used to generate a Time-based One Time Password (TOTP).


### HOTP example
```gdscript
func _ready() -> void:
	var secret: PoolByteArray = "secret".to_ascii()
	var moving_factor: int = randi() % 1000
	var hotp_generator: HOTPGenerator = HOTPGenerator.new()
	var hotp: String = hotp_generator.generate_hotp(secret, moving_factor)
	print(hotp) # will print something similar to `370727`
```

### TOTP example
```gdscript
func _ready() -> void:
	var secret: PoolByteArray = "secret".to_ascii()
	var totp_generator: TOTPGenerator = TOTPGenerator.new()
	var totp: String = totp_generator.generate_totp(secret)
	print(totp_generator.secs_remaining) # will print something between 0 and 30
	print(totp)	# will print something similar to `183920`
```

### Running Tests
*Is this library really RFC compliant?*
You can run compliancy tests yourself!
```gdscript
func _ready() -> void:
	assert(HOTPTest.new().run())
	"""
	Count     HMAC-SHA-1                                   Hexadecimal    Decimal        HOTP
	0         cc93cf18508d94934c64b65d8ba7667fb7cde4b0     4c93cf18       1284755224     755224
	1         75a48a19d4cbe100644e8ac1397eea747a2d33ab     41397eea       1094287082     287082
	2         0bacb7fa082fef30782211938bc1c5e70416ff44     82fef30        137359152      359152
	3         66c28227d03a2d5529262ff016a1e6ef76557ece     66ef7655       1726969429     969429
	4         a904c900a64b35909874b33e61c5938a8e15ed1c     61c5938a       1640338314     338314
	5         a37e783d7b7233c083d4f62926c7a25f238d0316     33c083d4       868254676      254676
	6         bc9cd28561042c83f219324d3c607256c03272ae     7256c032       1918287922     287922
	7         a4fb960c0bc06e1eabb804e5b397cdc4b45596fa     4e5b397        82162583       162583
	8         1b3c89f65e6c9e883012052823443f048b4332db     2823443f       673399871      399871
	9         1637409809a679dc698207310c8c7fc07290d9e5     2679dc69       645520489      520489
	"""

	assert(TOTPTest.new().run())
	"""
	Count     Unix Time    T value (Hex)      TOTP
	0         59           0000000000000001   94287082
	1         1111111109   00000000023523ec   07081804
	2         1111111111   00000000023523ed   14050471
	3         1234567890   000000000273ef07   89005924
	4         2000000000   0000000003f940aa   69279037
	"""
```

## Base32
Here some example usage of the Base32 class
```gdscript
func _ready() -> void:
	var str: String = "base32encoding"
	var base32_byte: PoolByteArray = Base32.encode(str.to_ascii())
	print(base32_byte.get_string_from_ascii()) 
	# will print 'MJQXGZJTGJSW4Y3PMRUW4ZY='
	var back_val: String = Base32.decode(base32_byte.get_string_from_ascii())
	print(back_val)
	# will print 'base32encoding'
```
# Using Marco

This is a guide for how to use Marco to attack a key fob, and the type of info you can expect to get from the key fob when the attack is successful.  

## Types of Attacks

Marco is capable of carrying out two types of attacks, both of which exploit the four-way handshake: A zero-knowledge identification attack, and a targeted-tracking attack.  

### Zero-Knowledge Identification Attack

The first attack is called a zero-knowledge identification attack, because the attacker does not need to know any specific information about the target vehicle to carry out the attack.  This attack targets the acknowledgement message of the key fob by rapidly generating a wake-up message.  If the attacker has a large library of wake-up messages, they can use this attack to identify the make and model of a vehicle, even if they know nothing about the target vehicle before the attack began.  This is because both the wake-up and acknowledgement messages are static messages.  This means these messages are the same for all vehicles of a given make, model, and generation.  In the case of a generation 3 Toyota Prius, every prius key fob will generate an acknowledgment message whenever they receive the wake up message `ffeaba`.  It doesn't matter how often you transmit this message or if the key fob is inside or outside the car, it will always respond.  This is what allows the attacker to attack a key fob without knowing what type of key fob the victim has ahead of time, as long as the attacker has a large enough library of wake-up messages, they will eventually discover the one that works, thus allowing them to identify the target without any previous knowledge of that specific key fob.  

This attack has two main flaws.  The first is that because all vehicles of a given make, model, and generation will respond to the same wake-up message, it is unable to tell these vehicles apart when it receives multiple responses.  This is because the acknowledgement message is also a static message, and thus all key fobs will respond with the same message.  For example, say an attacker is trying to track two cars of the same make, model, and generation, but one is red and the other is blue.  The attacker knows the wake-up message to transmit, and can see that they get two acknowledgement messages for each transmission.  This means that while the attacker can know that they are following two vehicles of a given make, model, and generation, they cannot tell which response is for the red car, and which one is for the blue.  This means that if one of the cars were to suddenly turn left, and the other turned right, the attacker would not be able to identify which path each vehicle took using this attack.  

The second flaw has to do with how many vehicles are vulnerable to this attack.  While this attack does target the four-way handshake protocol, not all versions of the protocol use all four messages.  While the traditional implementation consists of a wake-up, acknowledgement, challenge, and response messages, there is an alternative implementation that uses only the challenge and response messages.  Of the seven vehicles tested so far, two of them used this alternative implementation.  Because the alternative implementation doesn't use a wake-up or acknowledgement message, those key fobs are immune to this attack.  

### Targeted-Tracking Attack

This attack works by targeting the response message in the four-way handshake protocol.  To do this, the attacker generates both a wake-up and challenge message, mimicing the vehicle the key fob is paired to.  Because the challenge message contains the vehicles unique identifier, only the key fob that is paired to that vehicle will generate a response message.  This solves one of the problems with the previous attack in that know the attacker can know the specific key fob that is replying, even if there are other fobs of the same make and model nearby.  Additionally, because this attack targets the response message, this attack will work on key fobs that use both the traditional and alternative implementation of the four-way handshake protocol.  

These advantages do come at a price though, because the challenge message contains the unique identifier of the specific vehicle that generated it.  This means that the attacker must either be able to reverse aand generate  the vehicle identifier, or be able to record the challenge message generated by the vehicle.  Unlike some other attacks, a single captured challenged message will always be able to force the key fob to respond.  This is because passive keyless entry systems do not use any form of rolling code.  This makes capturing an instance of the challenge message by far the easiest way to collect the data needed for this attack.  

One other unique aspect of the targeted-tracking attack is that an attacker will force any LED on the key fob to go off every time the transmit a message.  In the zero-knowledge idetification attack, the attacker had the advantage of absolute stealth, because the led on most key fobs are tied to cryptographic chip within the key fob.  Because the fob generates a static response when hit with a wake-up message, that chip is never called and the LED will never turn on.  The response message does use the crypto chip on the fob to generate part of the message, and thus the key fob will light up every time a message is transmitted.  Thankfully in the real world, most people keep their key fob in their pocket or purse, so the odds of them noticng the LED going off is slim.  But it does cause this attack to be a great party trick.  
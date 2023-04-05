====== how to use =======

1. Get an MIT certificate. Even if you already have one "installed",
	you'll probably want a new copy. Open a browser and head to
	https://ist.mit.edu/certificates, and click 'get your MIT
	personal certificate' + follow the instructions onscreen.
	At some point you'll select an import password (remember this),
	and then you'll get a .p12 folder you can download to your machine.

2. Head back to https://ist.mit.edu/certificates, and get a copy of
	the MIT certificate authority.

3. Prepare the .p12 file for use with bundle. In a command prompt, navigate
	to the root of this repository and run the following:

	./scripts/splitp12.sh /path/to/your/p12/file importpassword

	Provided you get your input password right, you'll wind up with
	a .pem and .key file in your working directory. These are your
	public and private keys, respectively - protect the private key
	and don't share it with anyone!

4. Prepare the MIT CA for use with bundle. In the same directory you were in
	for step 3, run:

	./scripts/decodecrt.sh /path/to/your/mitca.crt

	You'll wind up with mitcca.pem in your working directory.

5. You're ready to go!

The build system overrides a handful of built-in Python functionalities.
You should know about these before you write a test script. The implementation
of each modification can be found in worker/api.py, but is briefly summarized here:

print(): ONLY ACCEPTS STRING ARGUMENTS. Prints to the client machine (e.g. your laptop) rather
than the worker machine's console.

readline(): REPLACES INPUT. Takes no arguments, and returns a string argument read from the
client machine (e.g. your laptop).

save(): Sends a file on the remote machine to your device. All files which are not save()d will
not appear on your machine after the build script has finished running. For instance, if you
are building a Vivado bitstream, you may not want to save 'vivado.log', but you may want to
save 'out.bit' - to do this, simply add `save(out.bit)` to the end of the build script.

Exceptions: Raising an uncaught exception (or calling the API function error(description)
will print an error message to the client machine before terminating the script as normal.


### Jil

A fast JSON (de)serializer, built on [Sigil](https://github.com/kevin-montrose/Sigil) with a number of somewhat crazy optimization tricks.

Forked from https://github.com/kevin-montrose/Jil, to ensure the DateTime property is deserialized to LocalTime, not the UTC time.

Just add some code in InlineDeserializer.cs ==> ReadNewtosoftDateTime(), before last "ExpectQuote"，add:

	var toLocalTime = typeof(DateTime).GetMethod("ToLocalTime");
        using (var loc = Emit.DeclareLocal<DateTime>())
        {
            Emit.StoreLocal(loc); // TextWriter
            Emit.LoadLocalAddress(loc); // TextWriter DateTime*
            Emit.Call(toLocalTime); // TextWriter DateTime
            Emit.StoreLocal(loc); // TextWriter
            Emit.LoadLocalAddress(loc); // TextWriter DateTime*
        }
then the output datetime will be local datetime, not the UTC's.

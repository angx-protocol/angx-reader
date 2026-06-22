# Proposal

*A potential stack for angx-reader.*

---

angx-reader runs alongside an ANGX client, on whatever device that
client already runs on — a steward's laptop or a base's persistent
device. It reads feeds the client has already replicated. It does not
participate in replication, transport, or notification — those belong
to ANGX and to whatever bridge carries its feeds.

---

**1. Source** — angeliaX feeds, already replicated locally by the
client. The reader watches `feeds/` for new signals. It opens nothing
over the network itself; it reads what Hypercore has already written
to disk.

**2. Matching** — `paraphrase-multilingual-MiniLM-L12-v2`, or
equivalent. Multilingual embedding model. Runs on CPU via ONNX
Runtime. Downloaded once at setup. Converts signal text into a vector
— two signals about the same problem land near each other regardless
of language, with no translation layer. Zero internet, zero cost per
query.

**3. Rules** — each new signal is passed to the embedding model, the
resulting vector stored in `vectors/`, and searched for nearby
matches. Physical provisions — food, water, shelter, energy — are
distance-filtered against the node's location field. Informational
provisions follow the same rule when they require presence, and match
globally when they do not. Learning signals match globally. Recent
signals rank higher in output. The underlying record never changes.

**4. Output** — a match is a pointer to two existing signals, nothing
more. The reader hands matches to whatever notification path the
local client already uses. It defines no transport or messaging
protocol of its own.

---

Every component is open source, self-hostable, and replaceable.
No vendor dependency. The right implementation of any layer may
look different from what is proposed here.

---

*angeliaX*

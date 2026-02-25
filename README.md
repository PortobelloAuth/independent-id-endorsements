# Independent Identity Endorsements

TODO: switch name to focus on endorsements, since both an authorization grant
and a witness of a relationship can be described as an endorsement

The Independent Identity Endorsements specification defines a format for
describing a legal document such as a contract or a witness of a birth
certificate or other relationship that conveys authorization. Identity
Endorsements may directly convey authorization information, as in the case of an
authorized representative, or implicit authorization information, as in the case
of an endorsement indicating that an identity represents the parent or guardian
of another (child) identity. Importantly, endorsements are associated with an
Identity but they are not the identity itself.

## Companion Specifications

Independent Identity Endorsements is intended to be used with the
[Open Person Matching](/PortobelloAuth/open-person-matching) and
[Open Independent ID](/PortobelloAuth/open-indepentent-id) specifications to
create a digital Identity, Authentication, Authorization, and Privacy framework
that allows individuals control of their own identity and accounts, including
delegated, limited access by human and automated agents, without account sharing
and other insecure practices.

## Draft Status

Please note that this repository and specification is currently in **DRAFT**
status. It is **not** yet licensed openly and is **not** considered fit for use.
This draft is published and licensed so that it may be reviewed and
recommendations for changes obtained.

## Motivations

Posts about AI agents fill your social media feed. Health and financial records
access is hotly debated policy. But we already have a longstanding legal
framework that supports all of this; we just need our technology to support it.

4000 years ago an Egyptian Pharoah delegated authority to Joseph. Today we often
write checks with which a signatory authorizes a person to transfer money from
their bank account to another. Professional athletes and entertainers often
employ legal agents to help them negotiate contracts. A parent or guardian is
authorized to act on behalf of their children. None of these agent relationships
are confined to a specific authorizing organization. Identity, authentication,
authorization, agency, and privacy are every day foundational principles for how
we interact with the world.

However, as digital technologies developed, computer and network accounts
focused specifically on authenticating and authorizing individuals within the
scope of a system or service. As a result, each of us may have as many as one
hundred of different accounts, with associated credentials to keep safe, across
the various services we use in our daily life. This proliferation of credentials
and poor support for authorizing agents encourages the insecure practice of
sharing credentials - sometimes facilitated by the use of password managers. But
this risks stolen credentials, grants implicit all-or-nothing authorization, and
works poorly with multi-factor authentication precisely because it is contrary
to how identity, authentication, authorization, agency, and privacy are intended
to work.

So, how do we enable a mother to collect her child's medical records? How do we
enable your AI agent to filter and organize your email, but not randomly delete
all of it? Not by giving them our identities! We do it by authorizing them to
act on our behalf. Independent Identity Endorsements are cryptographically
signed JSON documents that are associated with
[Open Independent Identities](/PortobelloAuth/open-indepentent-id). They
indicate relevant information that we know about an identity and what,
optionally, we intend them to be able to do or not do. You create an identity
for your AI agent and authorize it to move, but not delete your email. Now your
email services can tell the difference between you and your agent and act
accordingly. We know and document the mother's relationship with her child. We
endorse her identity with that relationship. Now health care providers can
verify that endorsement, apply existing law accordingly, and give that mother
access to her child's records.

## High Level Description

A Independent Identity Endorsement is a compound document consiting of:

- a document showing the text of the signed document sent directly as a data URL
  or referenced by URL (or similar mechanism); In simple cases this may be a tet
  document, however this may often take the form of a signed PDF (or similar
  image format) and annotated signature representations or scanned signatures
- a digital document summary consisting of:
  - Granting party identifier (when relevant)
  - Agent identifier
  - verified claims / grants / authorizations
  - conditions
- granting party signature (when relevant)
- agent signature
- witnessing party signatures
- Granting party and Agent signatures must cryptographically sign the full
  content of the unsigned document
- Witenssing party signatures must cryptographically sign the full content of
  the document including granting party and agent signatures, but not including
  other witness signatures

### Signatures

All digital signatures used in a Digital Authorization Document must be
non-repudiatable at the time of signing. Further, the document should be
periodically checked and signed using then current non-repudiatable methods in
order to ensure that older documents cannot be readly forged. A document that
cannot be verified in this manner must be verified to the satifaction of all
relevant parties through other means, which may prove difficult and time
consuming.

### Conceptual Example Documents

These example documents convey the type of information contained in Digital
Authorization Documents, not necessarily their exact structure. Indeed, much of
this information will actually be conveyed via specialized access tokens or
[Open Person Matching](/PortobelloAuth/open-person-matching) demographic hashes
in order to preserve the privacy of identified individuals as much as is
reasonable.

#### Agent Authorization

```json
{
  "subject": "Alfred Pennyworth",
  "by": "Bruce Wayne",
  "role": "legal agent/attorney",
  "to": "Bruce Wayne"
}
```

#### Parent or Gaurdian Attestation

```json
{
  "subject": "Fred Flintstone",
  "by": "Bedrock Notary",
  "role": "parent/guardian",
  "to": "Pebbles Flintstone",
  "verified_by": [{
    "document": "birth certificate"
  }, {
    "document": "tax return"
  }],
  "conditions": {
    "before": "01/01/1980"
  }
}
```

#### Negative Authorization

```json
{
  "subject": "Dominic Badguy",
  "by": "Kermit the Frog",
  "role": "denied access",
  "to": "Kermit the Frog",
  "verified_by": [{
    "document": "restraining order"
  }]
}
```

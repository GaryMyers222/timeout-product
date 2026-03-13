
# Invite State Machine

The Invite State Machine governs how users are invited to join a TimeOut circle and how the system resolves conflicts when multiple invitations exist.

This logic ensures that:

- invitations remain friend-origin and trusted
- users join the correct circle
- accidental duplicate circles are avoided
- legitimate multiple circles are still possible

---

# Invite Types

## Join Invite

A **Join Invite** is sent when a user invites a nearby trusted friend to join their circle.

Properties:

- invite_type = join
- invitee should join the inviter’s circle
- invitation is matched primarily by **phone number**

---

## Share Invite

A **Share Invite** is sent when a user shares TimeOut with someone outside the local circle.

Properties:

- invite_type = share
- invitee should start their own circle
- invite should not automatically attach to the inviter’s circle

---

# Invite States

## draft

The invite has been prepared but not yet sent.





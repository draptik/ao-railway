# ao-railway

A library for Railway Oriented Programming with Java.

See Scott Wlaschin's talk on the subject for more information: [Scott Wlaschin - Railway Oriented Programming - error handling in functional languages](https://vimeo.com/97344498 "Scott Wlaschin - Railway Oriented Programming - error handling in functional languages").

# Example

    public Result<User> changePassword(
        Username username, Password oldPasswort, Password newPassword)
    {
        return Result.combine(
            Result.of(username, "Username cannot be empty"),
            Result.of(oldPassword, "Old password cannot be empty"),
            Result.of(newPassword, "New password cannot be empty"))
        .onSuccess(() -> userRepo.find(username))
        .ensure(user -> user.isCorrectPassword(oldPassword), "Invalid password")
        .onSuccess(user -> user.changePassword(newPassword))
        .onSuccess(user -> userRepo.update(user))
        .onFailure(() -> logger.error("Password could not be changed"))
        .map(user -> user);
    }


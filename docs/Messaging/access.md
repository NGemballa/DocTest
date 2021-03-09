# Access matrix

<table class="wrapped">
    <tbody>
    <tr>
        <th>Resource</th>
        <th>Action</th>
        <th>Allowed</th>
    </tr>
    <tr>
        <td>UserGroup</td>
        <td>Create</td>
        <td>ADMIN, STAFFING</td>
    </tr>
    <tr>
        <td>UserGroup</td>
        <td>Update</td>
        <td>ADMIN, STAFFING</td>
    </tr>
    <tr>
        <td>UserGroup</td>
        <td>Delete</td>
        <td>ADMIN, STAFFING</td>
    </tr>
    <tr>
        <td>UserGroup</td>
        <td>Add members</td>
        <td>ADMIN, STAFFING</td>
    </tr>
    <tr>
        <td>UserGroup</td>
        <td>Remove members</td>
        <td>ADMIN, STAFFING</td>
    </tr>
    <tr>
        <td>Livechat</td>
        <td>Create</td>
        <td>ADMIN</td>
    </tr>
    <tr>
        <td>Livechat</td>
        <td>Update</td>
        <td>ADMIN</td>
    </tr>
    <tr>
        <td>Livechat</td>
        <td>Delete</td>
        <td>ADMIN</td>
    </tr>
    <tr>
        <td>User</td>
        <td>Create</td>
        <td>Not implemented</td>
    </tr>
    <tr>
        <td>User</td>
        <td>Update</td>
        <td>Current user, ADMIN(only internal user), STAFFING(only internal user)</td>
    </tr>
    <tr>
        <td>User</td>
        <td>Delete</td>
        <td>Not implemented</td>
    </tr>
    <tr>
        <td>User</td>
        <td>Get groups list</td>
        <td>Current user</td>
    </tr>
    <tr>
        <td>User</td>
        <td>Get status</td>
        <td>Current user</td>
    </tr>
    <tr>
        <td colspan="1">Status</td>
        <td colspan="1">Update</td>
        <td colspan="1">SUPPORT</td>
    </tr>
    <tr>
        <td colspan="1">Room</td>
        <td colspan="1">Create</td>
        <td colspan="1">Current user (internal or every collaboration user) can create all room types except of support
            and &quot;live chat user&quot; (non internal) can only create rooms of type support)
        </td>
    </tr>
    <tr>
        <td colspan="1">Room</td>
        <td colspan="1">Invite</td>
        <td colspan="1">Current user (internal or every collaboration user) can invite any other (internal or
            collaboration) user into his new created room
        </td>
    </tr>
    <tr>
        <td colspan="1">Room</td>
        <td colspan="1">Update</td>
        <td colspan="1">Not implemented</td>
    </tr>
    <tr>
        <td colspan="1">Room</td>
        <td colspan="1">Delete</td>
        <td colspan="1">Not implemented</td>
    </tr>
    <tr>
        <td colspan="1">Room</td>
        <td colspan="1">Get</td>
        <td colspan="1">
            <ul>
                <li>Operator: Current user member of or room is a public or ADMIN for closed support rooms&nbsp;</li>
                <li>Customer: member of support room</li>
            </ul>
        </td>
    </tr>
    </tbody>
</table>

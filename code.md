---
layout: default
---
# Code Blocks #
# **SEC1332C:** Level up your Response Actions: 
## Hands-on Building Splunk SOAR Apps using the SOAR App Wizard ##

## _get_token code
```python
def _get_token(self, action_result, from_action=False, **kwargs):
        auth = HTTPBasicAuth(self._username, self._password)
        headers = {
            'Content-Type': 'application/json'
        } 
        url = self._base_url
        try:
            r = requests.request(url=
                "{}{}".format(url,'/auth'),
                # auth=(username, password),  # basic authentication
                headers=headers,
                verify=self._verify,
                auth=auth,
                method="GET",
                **kwargs
                )
        except Exception as e:
            return RetVal(
                action_result.set_status(
                    phantom.APP_ERROR, "Error Connecting to server. Details: {0}".format(str(e))
                ), resp_json
            )

        if r.status_code != 200:
            self.debug_print("Failed to fetch a session token!")
            self._session_id = None
            return action_result.get_status()
        
        self._session_id = r.text.replace('"', '')
        self.save_state(self._state)        
        return phantom.APP_SUCCESS
```

## initialize code
```python
def initialize(self):
        # Load the state in initialize, use it to store data
        # that needs to be accessed across actions
        self._state = self.load_state()

        # get the asset config
        config = self.get_config()
        """
        # Access values in asset config by the name

        # Required values can be accessed directly
        required_config_name = config['required_config_name']

        # Optional values should use the .get() function
        optional_config_name = config.get('optional_config_name')
        """

        self._base_url = config.get('host')
        self._username = config.get('username')
        self._password = config.get('password')
        self._verfiy = config.get('verify')

        return phantom.APP_SUCCESS
```

## _handle_test_connectivity code
```python
def _handle_test_connectivity(self, param):
        # Add an action result object to self (BaseConnector) to represent the action for this param
        action_result = self.add_action_result(ActionResult(dict(param)))

        # NOTE: test connectivity does _NOT_ take any parameters
        # i.e. the param dictionary passed to this handler will be empty.
        # Also typically it does not add any data into an action_result either.
        # The status and progress messages are more important.

        self.save_progress("Connecting to endpoint")
        # make rest call
        ret_val, response = self._make_rest_call(
            '/test', action_result, params=None, headers=None
        )
        if phantom.is_fail(ret_val):
            # the call to the 3rd party device or service failed, action result should contain all the error details
            # for now the return is commented out, but after implementation, return from here
            self.save_progress("Test Connectivity Failed.")
            # return action_result.get_status()
```



## _handle_getusers code
```python
def _handle_getusers(self, param):
        # Implement the handler here
        # use self.save_progress(...) to send progress messages back to the platform
        self.save_progress("In action handler for: {0}".format(self.get_action_identifier()))

        # Add an action result object to self (BaseConnector) to represent the action for this param
        action_result = self.add_action_result(ActionResult(dict(param)))

        # Access action parameters passed in the 'param' dictionary

        # Required values can be accessed directly
        # required_parameter = param['required_parameter']

        # Optional values should use the .get() function
        # optional_parameter = param.get('optional_parameter', 'default_value')

        # make rest call
        ret_val, response = self._make_rest_call(
            '/getusers', action_result, params=None, headers=None
        )
        if phantom.is_fail(ret_val):
            # the call to the 3rd party device or service failed, action result should contain all the error details
            # for now the return is commented out, but after implementation, return from here
            # return action_result.get_status()
            pass
        
        # Now post process the data,  uncomment code as you deem fit

        # Add the response into the data section
        
        data=response["data"]
        number_of_results=response["number_of_results"]
        num_pages=response["num_pages"]
        results={'data':[]}
        for i in range(int(num_pages)):
            uri = '/getusers?page={}'.format(i)
            ret_val, response = self._make_rest_call(
                uri, action_result, params=None, headers=None
            )
            data=response["data"]
            for item in data:
                results['data'].append(item)
                
        action_result.add_data(results)

        # Add a dictionary that is made up of the most important values from data into the summary
        # summary = action_result.update_summary({})
        # summary['num_data'] = len(action_result['data'])

        # Return success, no need to set the message, only the status
        # BaseConnector will create a textual message based off of the summary dictionary
        return action_result.set_status(phantom.APP_SUCCESS)

        # For now return Error with a message, in case of success we don't set the message, but use the summary
        #return action_result.set_status(phantom.APP_ERROR, "Action not yet implemented")
```

## _handle_getdevices code
```python
def _handle_getdevices(self, param):
        # Implement the handler here
        # use self.save_progress(...) to send progress messages back to the platform
        self.save_progress("In action handler for: {0}".format(self.get_action_identifier()))

        # Add an action result object to self (BaseConnector) to represent the action for this param
        action_result = self.add_action_result(ActionResult(dict(param)))

        # Access action parameters passed in the 'param' dictionary

        # Required values can be accessed directly
        # required_parameter = param['required_parameter']

        # Optional values should use the .get() function
        status = param.get('status', None)
        if status:
            status_query="&status={}".format(status)
            ret_val, response = self._make_rest_call(
               '/getdevices?{}'.format(status_query), action_result, params=None, headers=None
            )
        else:
            status_query=None
            ret_val, response = self._make_rest_call(
               '/getdevices', action_result, params=None, headers=None
            )            
        # make rest call


        if phantom.is_fail(ret_val):
            # the call to the 3rd party device or service failed, action result should contain all the error details
            # for now the return is commented out, but after implementation, return from here
            # return action_result.get_status()
            pass

        # Now post process the data,  uncomment code as you deem fit

        data=response["data"]
        number_of_results=response["number_of_results"]
        num_pages=response["num_pages"]
        results={'data':[]}
        if status_query:
            data=response["data"]
            for item in data:
                results['data'].append(item)
        else:
            for i in range(int(num_pages)):
                uri = '/getdevices?page={}'.format(i)
                ret_val, response = self._make_rest_call(
                    uri, action_result, params=None, headers=None
                )
                data=response["data"]
                for item in data:
                    results['data'].append(item)
                
        action_result.add_data(results)
        # Add the response into the data section


        # Add a dictionary that is made up of the most important values from data into the summary
        # summary = action_result.update_summary({})
        # summary['num_data'] = len(action_result['data'])

        # Return success, no need to set the message, only the status
        # BaseConnector will create a textual message based off of the summary dictionary
        return action_result.set_status(phantom.APP_SUCCESS)

        # For now return Error with a message, in case of success we don't set the message, but use the summary
        #return action_result.set_status(phantom.APP_ERROR, "Action not yet implemented")
```

## _handle_containdevice code
```python
def _handle_containdevice(self, param):
        # Implement the handler here
        # use self.save_progress(...) to send progress messages back to the platform
        self.save_progress("In action handler for: {0}".format(self.get_action_identifier()))

        # Add an action result object to self (BaseConnector) to represent the action for this param
        action_result = self.add_action_result(ActionResult(dict(param)))

        # Access action parameters passed in the 'param' dictionary

        # Required values can be accessed directly
        hostname = param['hostname']

        # Optional values should use the .get() function
        # optional_parameter = param.get('optional_parameter', 'default_value')

        # make rest call
        ret_val, response = self._make_rest_call(
            '/containdevice', action_result, params=None, headers=None
        )

        if phantom.is_fail(ret_val):
            # the call to the 3rd party device or service failed, action result should contain all the error details
            # for now the return is commented out, but after implementation, return from here
            # return action_result.get_status()
            pass

        # Now post process the data,  uncomment code as you deem fit

        # Add the response into the data section
        action_result.add_data(response)

        # Add a dictionary that is made up of the most important values from data into the summary
        # summary = action_result.update_summary({})
        # summary['num_data'] = len(action_result['data'])

        # Return success, no need to set the message, only the status
        # BaseConnector will create a textual message based off of the summary dictionary
        # return action_result.set_status(phantom.APP_SUCCESS)

        # For now return Error with a message, in case of success we don't set the message, but use the summary
        return action_result.set_status(phantom.APP_ERROR, "Action not yet implemented")
```
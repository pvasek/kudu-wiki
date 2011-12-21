To turn profiling on for the Kudu Service, set the `enableProfiler` flag in the appSettings to true:

    <appSettings>
        <add key="enableProfiler" value="true" />
    </appSettings>

When profiling is enabled, every request (as well as other manually instrumented calls) is profiled and the time is stored in an xml file under the app root at `profiles\profile.xml`.
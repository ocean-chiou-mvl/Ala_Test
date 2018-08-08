#!/usr/bin/env groovy

import org.jenkinsci.plugins.pipeline.utility.steps.fs.FileWrapper
import com.cwctravel.hudson.plugins.extended_choice_parameter.ExtendedChoiceParameterDefinition
import java.net.URLEncoder
import java.util.regex.Matcher
import groovy.transform.Field
import javax.ws.rs.core.UriBuilder

def slave1Config = [
    "0" : ['SSH_ID': env.SOC1_0_SSH_ID,'APC_IP': env.SOC1_0_APC_IP,  'APC_SLOT': env.SOC1_0_APC_SLOT,'TARGET_IP': env.SOC1_0_TARGET_IP,'TCP_IP':env.SOC1_0_TCP_IP],
    "1" : ['SSH_ID': env.SOC1_1_SSH_ID,'APC_IP': env.SOC1_1_APC_IP,  'APC_SLOT': env.SOC1_1_APC_SLOT,'TARGET_IP': env.SOC1_1_TARGET_IP,'TCP_IP':env.SOC1_1_TCP_IP]
]
def slave2Config = [
    "0" : ['SSH_ID': env.SOC2_0_SSH_ID,'APC_IP': env.SOC2_0_APC_IP,  'APC_SLOT': env.SOC2_0_APC_SLOT,'TARGET_IP': env.SOC2_0_TARGET_IP,'TCP_IP':env.SOC2_0_TCP_IP],
    "1" : ['SSH_ID': env.SOC2_1_SSH_ID,'APC_IP': env.SOC2_1_APC_IP,  'APC_SLOT': env.SOC2_1_APC_SLOT,'TARGET_IP': env.SOC2_1_TARGET_IP,'TCP_IP':env.SOC2_1_TCP_IP],
    "2" : ['SSH_ID': env.SOC2_2_SSH_ID,'APC_IP': env.SOC2_2_APC_IP,  'APC_SLOT': env.SOC2_2_APC_SLOT,'TARGET_IP': env.SOC2_2_TARGET_IP,'TCP_IP':env.SOC2_2_TCP_IP]
]
def configMap = [ (env.SOC1_SLAVENAME) : slave1Config, (env.SOC2_SLAVENAME) : slave2Config]

def toDo = [
    /* Build with tests */
    [ name: '1098R20_Internal_E2e_Bics2_Nvme',              soc: '1098R20',  customer: 'Internal',      target: 'E2e_Bics2_Nvme',                configId: '0',  build: 'windows-build', test: env.SOC2_SLAVENAME],

    /* Build only */
    //Eldora
    [ name: '1093R21_Internal_E2e_tfx132',           soc: '1093R21',         customer: 'Internal',      target: 'E2e_tfx132',                    configId: '0',  build: 'windows-build' ],
    [ name: '1093R21_Internal_E2e_tfx132_Mst',       soc: '1093R21',         customer: 'Internal',      target: 'E2e_tfx132_Mst',                configId: '0',  build: 'windows-build' ],
    //Zao
    [ name: '1098_Internal_E2e_Bics2_Nvme',                soc: '1098',      customer: 'Internal',      target: 'E2e_Bics2_Nvme',                configId: '0',  build: 'windows-build'],
    [ name: '1098_Internal_E2e_Bics3_Nvme',                soc: '1098',      customer: 'Internal',      target: 'E2e_Bics3_Nvme',                configId: '0',  build: 'windows-build' ],
    [ name: '1098_Internal_E2e_Bics2_Nvme_4MediaSpaces',   soc: '1098',      customer: 'Internal',      target: 'E2e_Bics2_Nvme_4MediaSpaces',   configId: '0',  build: 'windows-build' ],
    [ name: '1098_Internal_E2e_Bics2_Nvme_Historylog',     soc: '1098',      customer: 'Internal',      target: 'E2e_Bics2_Nvme_Historylog',     configId: '0',  build: 'windows-build' ],
    [ name: '1098_Standard_E2e_Bics2',                     soc: '1098',      customer: 'Standard',      target: 'E2e_Bics2',                     configId: '0',  build: 'windows-build' ],


    //ZaoR20
    [ name: '1098R20_Internal_E2e_Bics2_Nvme_4MediaSpaces', soc: '1098R20',      customer: 'Internal',     target: 'E2e_Bics2_Nvme_4MediaSpaces',  configId: '0',   build: 'windows-build' ],
    [ name: '1098R20_Internal_E2e_Bics2_Nvme_Historylog',   soc: '1098R20',      customer: 'Internal',     target: 'E2e_Bics2_Nvme_Historylog',    configId: '0',   build: 'windows-build' ],
    [ name: '1098R20_Internal_E2e_Bics2_Sata',              soc: '1098R20',      customer: 'Internal',     target: 'E2e_Bics2_Sata',               configId: '0',   build: 'windows-build' ],
    [ name: '1098R20_Internal_E2e_Bics3_Nvme',              soc: '1098R20',      customer: 'Internal',     target: 'E2e_Bics3_Nvme',               configId: '0',   build: 'windows-build' ],
    [ name: '1098R20_Internal_Bics2_Nvme_Mst_Lite',         soc: '1098R20',      customer: 'Internal',     target: 'Bics2_Nvme_Mst_Lite',          configId: '0',   build: 'windows-build' ],
    [ name: '1098R20_Internal_E2e_Bics2_Nvme_Mst',          soc: '1098R20',      customer: 'Internal',     target: 'E2e_Bics2_Nvme_Mst',           configId: '0',   build: 'windows-build' ],
    [ name: '1098R20_Standard_Ramdrive0',                   soc: '1098R20',      customer: 'Standard',     target: 'Ramdrive0',                    configId: '0',   build: 'windows-build' ],
    [ name: '1098R20_Standard_E2e_Bics2',                   soc: '1098R20',      customer: 'Standard',     target: 'E2e_Bics2',                    configId: '0',   build: 'windows-build' ],

]

timestamps {
  stage("Build") {
    echo "After Build"
  }
  stage("Test") {
    //parallel testTasks
    echo "Test Code Here"
  }    
}

/**
 * Creates a new instance of BuildCombination based on the given FileWrapper
 * @param file The FileWrapper file that a BuildCombination should be created for
 * @return An instance of BuildCombination with the parameters found in the file
 */
BuildCombination fileToBuildCombination(FileWrapper file) {
  List path = file.path.tokenize('\\')
  new BuildCombination([
    target:    getFilenameWithoutExtension(file),
    soc:       path[-2],
    customer:  path[-3],
    buildSets: getSetsFromMakefile("_BUILD_SETS", file),
    testSets:  getSetsFromMakefile("_TEST_SETS", file)
  ])
}

/**
 * Read a given makefile from the slave and parse the found sets if it has any
 * @param setName The name of the set to look for, e.g. _BUILD_SET
 * @param file The FileWrapper file that should be parsed for sets
 * @return A list representing the sets found in the given file
 */
List<String> getSetsFromMakefile(String setName, FileWrapper file) {
  Matcher setMatcher = readFile(file.path) =~ /${setName}=(.*)/
  setMatcher ? stringAsList(setMatcher[0][1]) : []
}

/**
 * Extract just the filename without the extension for a give FileWrapper
 * @param file The FileWrapper file of which we want the filename
 * @return A String representing just the filename of the give FileWrapper
 */
String getFilenameWithoutExtension(FileWrapper file) {
  file.name.take(file.name.lastIndexOf('.'))
}

/**
 * Check if the given file is valid for our purposes
 * @param file The FileWrapper file to be checked for validity
 * @return True if it is a valid file, false otherwise
 */
boolean isValidFile(FileWrapper file) {
  !(file.name.startsWith("_") || file.name.toLowerCase().contains("common"))
}

/**
 * Turn string representation of List back into a List:
 * - Take the String value between the [ and ] brackets
 * - Split on , to get a List
 * - Convert each List item to a list entry (as a String)
 *
 * @param input A List as String, e.g. "[value1, value2]"
 * @return Returns the List interpertation from the String
 */
List<String> stringAsList(String input) {
  (input.length() > 2) ? input[1..-2].split(', ').collect {it as String} : []
}

/**
 * Print some formatted debug information about found buildCombinations and
 * the source control variables.
 * @param buildCombinations a list of BuildCombination objects
 * @param scmVars a map of source control variables
 * @param libraryClasses a map of library class instances
 * @param params a map of all the build parameters
 */
void printDebugInformation(List<BuildCombination> buildCombinations, Map scmVars, Map libraryClasses, Map params) {
  String debugInformation = "--- Details for build #${currentBuild.number} ---\n"

  debugInformation += "Git checkout variables:\n"
  scmVars.each { key, value ->
    debugInformation += " - $key: $value\n"
  }

  debugInformation += "Build combinations: ${buildCombinations.size()}\n"
  buildCombinations.each {
    debugInformation += " - ${it as String}\n"
    debugInformation += "   Build Sets: ${it.buildSets}\n"
    debugInformation += "   Test  Sets: ${it.testSets}\n"
  }

  debugInformation += "Library classes: ${libraryClasses.size()}\n"
  libraryClasses.each { key, value ->
    debugInformation += " - $key\n"
  }

  debugInformation += "Params: ${params.size()}\n"
  params.each { key, value ->
    debugInformation += " - $key: $value\n"
  }


  echo debugInformation
}

/**
 * Create a build discarder strategy based on the fact that we keep builds only
 * 7 days before deleting them.
 * @return Returns the build discarder strategy instance
 */
def createBuildDiscarderStrategy() {
  return [$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', daysToKeepStr: '7']]
}

/**
 * Create an ExtendedChoiceParameterDefinition for a list of targets.
 * @param what a description of that this parameter is used for, e.g. build
 * @param buildCombinations a list of BuildCombination objects
 * @param scmVars The source code management variables.
 */
ExtendedChoiceParameterDefinition createTargetChoices(String what, List<BuildCombination> buildCombinations, Map scmVars) {
  return new ExtendedChoiceParameterDefinition(
    what,
    "PT_MULTI_SELECT",
    buildCombinations.collect { buildCombination -> buildCombination as String }.join(','),
    null, null, null, null, null, null, null,
    getDefaultBuildTargets(buildCombinations, scmVars),
    null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, false, false,
    buildCombinations.size(),
    "Select one, or multiple, targets to $what.",
    ","
  )
}

/**
 * Agnostically Executes a script on Windows or Linux
 * @param script the script to Execute
 * @param returnStdout whether to return the output from Stdout or Agnostically
 */
String shell(String script, boolean returnStdout = false) {
    isUnix() ?
      sh(script: "$script", returnStdout: returnStdout) :
      powershell(script: "$script", returnStdout: returnStdout)?.trim()
}

/**
 * Execute a git show with a given format to get details. For examples of formats
 * refer to https://git-scm.com/docs/pretty-formats.
 * @param format the format, for example %h for an abbreviated commit hash
 * @return Returns the output of the requested format
 */
String gitDetails(String format) {
  shell("git --no-pager show -s --format=${format}", true)
}

/**
 * Submit a given build to the ATF.
 * @param buildCombination The BuildCombination of the to be submitted build.
 * @param scmVars The source code management variables.
 * @param atfUri The URI used to submit this target to the ATF.
 */
void submitToAtf(BuildCombination buildCombination, Map scmVars, URI atfUri) {
    String atfResponse = atfUri.toURL().getText()
    String testLinks = parseAtfResponse(atfResponse)
    echo "Submitted to ATF using '${atfUri.toString()}', got response: '${atfResponse}', extracted as URLs '${testLinks}'"
    addGerritComment("${buildCombination as String} - ATF Links: ${testLinks}", scmVars)
}

/**
 * Convert an atf response into a String representation of a Map of links.
 * @param atfResponse The response of the ATF on the submit.
 * @return Returns a Map as String e.g. [test1:link1, test2:link2].
 */
@NonCPS
String parseAtfResponse(String atfResponse) {
  try {
    return new XmlSlurper()
      .parseText(atfResponse)           // Parse the return message of the ATF
      .Tests                            // It contains a list called Tests
      .children()                       // The items in this list are the children
      .inject([:]) { map, test ->       // Inject an empty map and use the map and test
        map + [(test.Name): test.Link]  // In a closure transform the test to key values
      }
      .toMapString()                    // And return it all as a String
  } catch(e) {
    echo "There was a problem parsing the ATF response: ${e}."
    return null
  }
}

/**
 * Create a link to submit a given build to the ATF.
 * @param buildCombination The BuildCombination of the to be submitted build.
 * @param atfUri The URI used to submit this target to the ATF.
 */
void createSubmitToAtfLink(BuildCombination buildCombination, URI atfUri) {
  manager.createSummary("monitor.gif").appendText("<a href=\"${atfUri.toString()}\" target=\"_blank\">${buildCombination as String} - Add label to ATF </a>", false)
}

/**
 * Generate a submit uri for the ATF through a UriBuilder.
 * @param buildCombination The BuildCombination of the to be submitted build.
 * @param scmVars The source code management variables.
 * @param bin A link to the binary file produced by the build.
 * @param tokens A link to the tokens file produced by the build.
 * @return An instance of an URI for the ATF submit.
 */
URI generateAtfUri(BuildCombination buildCombination, Map scmVars, String bin, String tokens) {
  UriBuilder
        .fromPath("http://10.85.130.53/php/Jenkins.php")
        .queryParam("bin",         "${env.BUILD_URL}${bin}")
        .queryParam("TOKENS",      tokens ? "${env.BUILD_URL}${tokens}" : "")
        .queryParam("TARGET",      buildCombination.toString("_"))
        .queryParam("ATF_PROJECT", env.JOB_NAME.split("/").first())
        .queryParam("REVISION",    libraryClasses.ScmVarsUtils.getRevision(scmVars))
        .queryParam("BRANCH",      libraryClasses.ScmVarsUtils.getBranch(scmVars))
        .queryParam("AUTHOR",      libraryClasses.ScmVarsUtils.getUsername(scmVars))
        .build()
}

/**
 * Change the automated flag in the ATF uri and set the test set.
 * @param atfUri The atfUri, an instance of URI.
 * @param testSet A string which test set should automatically be ran.
 * @return An instance of an URI for the ATF submit.
 */
URI setTestSet(URI atfUri, String testSet) {
  UriBuilder
    .fromUri(atfUri)
    .queryParam("TEST_SET", testSet)
    .build()
}

/**
 * Add a comment to Gerrit for the current build, will only do something if this
 * build is triggered by Gerrit.
 * @param comment The comment to be posted to Gerrit.
 * @param scmVars The source code management variables.
 */
void addGerritComment(String comment, Map scmVars) {
  if (libraryClasses.ScmVarsUtils.isReference(scmVars)) {
    shell "ssh -p 29418 vgitcentral.marvell.com gerrit review --notify NONE -m \"'$comment'\" ${libraryClasses.ScmVarsUtils.getCommitAbbreviated(scmVars)}"
  }
}

/**
 * Get the active test set for this build.
 * @param buildCombination The BuildCombination of the to be submitted build.
 * @param scmVars The source code management variables.
 * @return The active test set or null if no test set is active.
 */
String getActiveTestSet(BuildCombination buildCombination, Map scmVars) {
  // Groovy will return the first Groovy Truth element in the list or null if
  // the intersect is null or the list is empty
  getPossibleTestSets(scmVars).intersect(buildCombination.testSets)?.find()
}

/**
 * Get the possible test sets for this build.
 * @param scmVars The source code management variables.
 * @return A list of possible test sets for this build. Can be an empty list.
 */
List<String> getPossibleTestSets(Map scmVars) {
  List possibleTestSets = []

  if (libraryClasses.ScmVarsUtils.isReference(scmVars)) possibleTestSets << "Precommit"
  if (libraryClasses.ScmVarsUtils.isMaster(scmVars))    possibleTestSets << "Master"

  return possibleTestSets
}

/**
 * Get the build set for this build.
 * @param scmVars The source code management variables.
 * @return The build set for this build. Could be empty.
 */
String getBuildSet(Map scmVars) {
  String buildSet = ""

  if (libraryClasses.ScmVarsUtils.isReference(scmVars)) buildSet = "Precommit"
  if (libraryClasses.ScmVarsUtils.isMaster(scmVars)) buildSet = "Master"

  return buildSet
}

/**
 * True if the build should automatically be tested by the ATF.
 * @param buildCombination The BuildCombination of the to be submitted build.
 * @param scmVars The source code management variables.
 * @return True for automatic testing, false otherwise
 */
boolean shouldAutomatedAtfTest(BuildCombination buildCombination, Map scmVars) {
  libraryClasses.ScmVarsUtils.isMaster(scmVars) && buildCombination.testSets.contains("Master")
}

/**
 * Create an string representing the builds for a list of targets.
 * @param buildCombinations a list of BuildCombination objects
 * @param scmVars The source code management variables.
 */
String getDefaultBuildTargets(List<BuildCombination> buildCombinations, Map scmVars) {
  return buildCombinations.findAll { buildCombination ->
    // If the buildset is empty use precommit as default
    buildCombination.buildSets.contains(getBuildSet(scmVars) ?: "Precommit")
  }.collect { buildCombination ->
    buildCombination as String
  }.join(',')
}

/**
 * Determine if a commit message is according to our specifications.
 * @return True if the commit message is formatted correctly, false otherwise.
 */
boolean isValidCommitMessage() {
  return (gitDetails("%B") ==~ /(?s)^[0-9A-Z]{5} \[ALAMERE-[0-9]+\] .*/)
}

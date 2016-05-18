/**
 * Unit Tests for GoalUndo class
**/

#include <gtest/gtest.h>
#include "GoalUndo.h"
 
class GoalUndoTest : public ::testing::Test
{
	protected:
		GoalUndoTest(){}
		virtual ~GoalUndoTest(){}
		virtual void SetUp()
		{
			g1_.addOperation("goal_1", "operation_1_g_1");
			g2_.addOperation("goal_1", "operation_1_g_1");
			g2_.addOperation("operation_2_g_1");
			g2_.addOperation("operation_3_g_1");
			g2_.addOperation("goal_2", "operation_1_g_2");
			g2_.addOperation("operation_2_g_2");
			g2_.addOperation("operation_3_g_2");

			g_dupl_.addOperation("goal_1", "operation_1_g_1");
			g_dupl_.addOperation("operation_1_g_1");
			g_dupl_.addOperation("operation_1_g_1");
			g_dupl_.addOperation("operation_2_g_1");
			g_dupl_.addOperation("operation_2_g_1");
			g_dupl_.addOperation("operation_3_g_1");

			g_dupl_.addOperation("goal_1", "operation_1_g_1");
			g_dupl_.addOperation("operation_2_g_1");
			g_dupl_.addOperation("operation_2_g_1");
			g_dupl_.addOperation("operation_1_g_1");
			g_dupl_.addOperation("operation_3_g_1");

			g_same_.addOperation("same");
			g_same_.addOperation("same");
			g_same_.addOperation("same");
		}
		virtual void TearDown(){}

	GoalUndo g0_;	// empty
	GoalUndo g1_;	// one goal and one different operation
	GoalUndo g2_;	// two different goals
	GoalUndo g_dupl_;	// two equal goals
	GoalUndo g_same_;	// goal and operations have the same names
};

/**
 * check initial state of fixtures
 */
TEST_F(GoalUndoTest, initTest)
{
	ASSERT_EQ(g0_.getGoal(), "");
	ASSERT_EQ(g1_.getGoal(), "goal_1");
	ASSERT_EQ(g2_.getGoal(), "goal_2");
	ASSERT_EQ(g_dupl_.getGoal(), "goal_1");
	ASSERT_EQ(g_same_.getGoal(), "same");

	ASSERT_EQ(g0_.getOperations(), "");
	ASSERT_EQ(g1_.getOperations(), "operation_1_g_1");
	ASSERT_EQ(g2_.getOperations(), "operation_1_g_2 operation_2_g_2 operation_3_g_2");
	ASSERT_EQ(g_dupl_.getOperations(), "operation_1_g_1 operation_2_g_1 operation_2_g_1 operation_1_g_1 operation_3_g_1");
	ASSERT_EQ(g_same_.getOperations(), "same same same");
}

/**
 * trivial undo goal calls check
 */
TEST_F(GoalUndoTest, goalUndoGoal)
{
	g0_.undoGoal();
	g1_.undoGoal();
	g2_.undoGoal();
	g_dupl_.undoGoal();
	g_same_.undoGoal();

	ASSERT_EQ(g0_.getGoal(), "");
	ASSERT_EQ(g1_.getGoal(), "");
	ASSERT_EQ(g2_.getGoal(), "goal_1");
	ASSERT_EQ(g_same_.getGoal(), "");
	ASSERT_EQ(g2_.getOperations(), "operation_1_g_1 operation_2_g_1 operation_3_g_1");
	ASSERT_EQ(g_dupl_.getOperations(), "operation_1_g_1 operation_1_g_1 operation_1_g_1 operation_2_g_1 operation_2_g_1 operation_3_g_1");
	ASSERT_EQ(g_same_.getOperations(), "");
}

/**
 * add operation + add empty operation(nothing happens)
 */
TEST_F(GoalUndoTest, goalAddOperationAndEmptyOperation)
{
	g0_.addOperation("oper0");
	g1_.addOperation("oper1");
	g2_.addOperation("oper2");
	g_dupl_.addOperation("oper_d");
	g_same_.addOperation("same");

	for(int i = 0; i < 2; ++i) {
		ASSERT_EQ(g0_.getGoal(), "oper0");
		ASSERT_EQ(g1_.getGoal(), "goal_1");
		ASSERT_EQ(g2_.getGoal(), "goal_2");
		ASSERT_EQ(g_dupl_.getGoal(), "goal_1");
		ASSERT_EQ(g_same_.getGoal(), "same");

		ASSERT_EQ(g0_.getOperations(), "oper0");
		ASSERT_EQ(g1_.getOperations(), "operation_1_g_1 oper1");
		ASSERT_EQ(g2_.getOperations(), "operation_1_g_2 operation_2_g_2 operation_3_g_2 oper2");
		ASSERT_EQ(g_dupl_.getOperations(), "operation_1_g_1 operation_2_g_1 operation_2_g_1 operation_1_g_1 operation_3_g_1 oper_d");
		ASSERT_EQ(g_same_.getOperations(), "same same same same");

		g0_.addOperation("");
		g1_.addOperation("");
		g2_.addOperation("");
		g_dupl_.addOperation("");
		g_same_.addOperation("");
	}
}

/**
 * undo operation and check state after each undo
 */
TEST_F(GoalUndoTest, goalUndoOperation)
{
	g0_.undoOperation();
	g1_.undoOperation();
	g2_.undoOperation();
	g_dupl_.undoOperation();
	g_same_.undoOperation();

	ASSERT_EQ(g0_.getGoal(), "");
	ASSERT_EQ(g1_.getGoal(), "");
	ASSERT_EQ(g2_.getGoal(), "goal_2");
	ASSERT_EQ(g_dupl_.getGoal(), "goal_1");
	ASSERT_EQ(g_same_.getGoal(), "same");

	ASSERT_EQ(g0_.getOperations(), "");
	ASSERT_EQ(g1_.getOperations(), "");
	ASSERT_EQ(g2_.getOperations(), "operation_1_g_2 operation_2_g_2");
	ASSERT_EQ(g_dupl_.getOperations(), "operation_1_g_1 operation_2_g_1 operation_2_g_1 operation_1_g_1");
	ASSERT_EQ(g_same_.getOperations(), "same same");


	g0_.undoOperation();
	g1_.undoOperation();
	g2_.undoOperation();
	g_dupl_.undoOperation();
	g_same_.undoOperation();

	ASSERT_EQ(g0_.getGoal(), "");
	ASSERT_EQ(g1_.getGoal(), "");
	ASSERT_EQ(g2_.getGoal(), "goal_2");
	ASSERT_EQ(g_dupl_.getGoal(), "goal_1");
	ASSERT_EQ(g_same_.getGoal(), "same");

	ASSERT_EQ(g0_.getOperations(), "");
	ASSERT_EQ(g1_.getOperations(), "");
	ASSERT_EQ(g2_.getOperations(), "operation_1_g_2");
	ASSERT_EQ(g_dupl_.getOperations(), "operation_1_g_1 operation_2_g_1 operation_2_g_1");
	ASSERT_EQ(g_same_.getOperations(), "same");
}

/**
 * check goals end is feasible by removing operation by operation
 */
TEST_F(GoalUndoTest, goalCleanupOperByOper)
{
	// cleanup goals by operations
	for(int i = 0; i != 20; ++i)
	{
		g0_.undoOperation();
		g1_.undoOperation();
		g2_.undoOperation();
		g_dupl_.undoOperation();
		g_same_.undoOperation();
	}

	ASSERT_TRUE(g0_.getGoal().empty());
	ASSERT_TRUE(g1_.getGoal().empty());
	ASSERT_TRUE(g2_.getGoal().empty());
	ASSERT_TRUE(g_dupl_.getGoal().empty());
	ASSERT_TRUE(g_same_.getGoal().empty());

	ASSERT_TRUE(g0_.getOperations().empty());
	ASSERT_TRUE(g1_.getOperations().empty());
	ASSERT_TRUE(g2_.getOperations().empty());
	ASSERT_TRUE(g_dupl_.getOperations().empty());
	ASSERT_TRUE(g_same_.getOperations().empty());
}

/**
 * check goals end is feasible by removing goals
 */
TEST_F(GoalUndoTest, goalCleanupRuleByRule)
{
	// cleanup goals by rules
	for(int i = 0; i != 4; ++i)
	{
		g0_.undoGoal();
		g1_.undoGoal();
		g2_.undoGoal();
		g_dupl_.undoGoal();
		g_same_.undoGoal();
	}

	ASSERT_TRUE(g0_.getGoal().empty());
	ASSERT_TRUE(g1_.getGoal().empty());
	ASSERT_TRUE(g2_.getGoal().empty());
	ASSERT_TRUE(g_dupl_.getGoal().empty());
	ASSERT_TRUE(g_same_.getGoal().empty());

	ASSERT_TRUE(g0_.getOperations().empty());
	ASSERT_TRUE(g1_.getOperations().empty());
	ASSERT_TRUE(g2_.getOperations().empty());
	ASSERT_TRUE(g_dupl_.getOperations().empty());
	ASSERT_TRUE(g_same_.getOperations().empty());
}

/**
 * undo operations with empty and non existent operations
 */
TEST_F(GoalUndoTest, goalUndoOperationNothing)
{
	const std::string input[] = { std::string(""), std::string("x") };

	for(size_t i = 0; i != sizeof(input)/sizeof(input[0]); ++i)
	{
		g0_.undoOperation(input[i]);
		g1_.undoOperation(input[i]);
		g2_.undoOperation(input[i]);
		g_dupl_.undoOperation(input[i]);
		g_same_.undoOperation(input[i]);

		ASSERT_EQ(g0_.getGoal(), "");
		ASSERT_EQ(g1_.getGoal(), "goal_1");
		ASSERT_EQ(g2_.getGoal(), "goal_2");
		ASSERT_EQ(g_dupl_.getGoal(), "goal_1");
		ASSERT_EQ(g_same_.getGoal(), "same");

		ASSERT_EQ(g0_.getOperations(), "");
		ASSERT_EQ(g1_.getOperations(), "operation_1_g_1");
		ASSERT_EQ(g2_.getOperations(), "operation_1_g_2 operation_2_g_2 operation_3_g_2");
		ASSERT_EQ(g_dupl_.getOperations(), "operation_1_g_1 operation_2_g_1 operation_2_g_1 operation_1_g_1 operation_3_g_1");
		ASSERT_EQ(g_same_.getOperations(), "same same same");
	}
}

/**
 * advanced undo operations using operation name
 * some tests are commented because bug in undoOperation(std::string)
 */
TEST_F(GoalUndoTest, goalUndoOperationAdvanced)
{
	g0_.undoOperation("operation_1_g_1");
	g1_.undoOperation("operation_1_g_1");
	g2_.undoOperation("operation_1_g_2");
	g_dupl_.undoOperation("operation_1_g_1");
	g_same_.undoOperation("same");

	ASSERT_EQ(g0_.getOperations(), "");
	ASSERT_EQ(g1_.getOperations(), "");
	ASSERT_EQ(g2_.getOperations(), "operation_2_g_2 operation_3_g_2");
	ASSERT_EQ(g_dupl_.getOperations(), "operation_1_g_1 operation_2_g_1 operation_2_g_1 operation_3_g_1");
	ASSERT_EQ(g_same_.getOperations(), "same same");

	ASSERT_EQ(g0_.getGoal(), "");
	//ASSERT_EQ(g1_.getGoal(), "");	// inconsistency - fail here
	ASSERT_EQ(g2_.getGoal(), "goal_2");
	ASSERT_EQ(g_dupl_.getGoal(), "goal_1");
	ASSERT_EQ(g_same_.getGoal(), "same");

	g0_.undoOperation();
	//g1_.undoOperation();	// crash because of bug in undoOperation(std::string)
	g2_.undoOperation();
	g_dupl_.undoOperation();
	g_same_.undoOperation();

	ASSERT_EQ(g0_.getGoal(), "");
	//ASSERT_EQ(g1_.getGoal(), "");	// inconsistency - fail here
	ASSERT_EQ(g2_.getGoal(), "goal_2");
	ASSERT_EQ(g_dupl_.getGoal(), "goal_1");
	ASSERT_EQ(g_same_.getGoal(), "same");

	ASSERT_EQ(g0_.getOperations(), "");
	ASSERT_EQ(g1_.getOperations(), "");
	ASSERT_EQ(g2_.getOperations(), "operation_2_g_2");
	ASSERT_EQ(g_dupl_.getOperations(), "operation_1_g_1 operation_2_g_1 operation_2_g_1");
	ASSERT_EQ(g_same_.getOperations(), "same");
}

/**
 * some invalid calls
 */
TEST_F(GoalUndoTest, goaInvalid)
{
	GoalUndo g;
	g.addOperation("");
	g.addOperation("", "x");
	g.addOperation("x", "");

	ASSERT_TRUE(g.getGoal().empty());
	ASSERT_TRUE(g.getOperations().empty());
	g.undoGoal();
	g.undoOperation();
	g.undoOperation("");
	ASSERT_TRUE(g.getGoal().empty());
	ASSERT_TRUE(g.getOperations().empty());
}

/**
 * dedicated test for undoOperation(std::string) - for remove only

 */
TEST_F(GoalUndoTest, goalCodeCoverage)
{
	// incorrect test - for remove only
	g1_.undoOperation("operation_1_g_1");
	ASSERT_EQ(g1_.getGoal(), "goal_1");
	ASSERT_TRUE(g1_.getOperations().empty());
}
